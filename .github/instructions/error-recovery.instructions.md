---
name: Error Recovery & Graceful Degradation
description: Patterns for handling failures, escalating, and recovering from errors
applyTo: "**/*.agent.md"
---

# Error Recovery & Graceful Degradation

When things fail, agents must degrade gracefully. This guide defines recovery patterns for common failures.

---

## Degradation Levels

When an error occurs, follow this escalation path:

### Level 1: Retry (Transient Errors)

**When:** Tool timeout, temporary network error, rate limit  
**Action:** Retry once with exponential backoff  
**Duration:** Wait 1-2 seconds, then retry

```
IF tool_call fails with timeout:
  → Wait 2 seconds
  → Retry with smaller scope (fewer files, simpler query)
  → If retry succeeds: Continue
  → If retry fails: Go to Level 2
```

**Example:**
```
semantic_search("broad query") → timeout
→ Wait 2s
→ Retry: semantic_search("narrower, more specific query")
→ Success: Continue with results
```

### Level 2: Fallback (Tool Unavailable)

**When:** Tool returns error, alternative tool available  
**Action:** Use different tool to achieve same goal  
**Success Rate:** 80%+ (most tasks have multiple tools)

```
IF primary tool fails:
  → Try alternative tool for same problem
  → If alternative succeeds: Continue
  → If alternative fails: Go to Level 3
```

**Examples:**

| Original Tool | Failed | Alternative | Use Case |
|:---|:---|:---|:---|
| `semantic_search` | timeout | `grep_search` | Finding specific keywords |
| `grep_search` | no matches | `file_search` + manual inspection | File doesn't exist |
| `read_file` | file not found | `file_search` first, then `read_file` | Wrong path |
| `replace_string_in_file` | pattern not found | Check file manually, verify pattern | String mismatch |

### Level 3: Escalate (Human Decision Needed)

**When:** Retry failed, fallback unavailable, agent at decision point  
**Action:** Handoff to prompt-critic with full context  
**Duration:** Immediate handoff

```
IF Level 1 (retry) AND Level 2 (fallback) both failed:
  → Handoff to prompt-critic
  → Include: What was attempted, why each failed, what decisions needed
  → Wait for human guidance
```

**Example escalation handoff:**
```
Handoff to prompt-critic:
"I attempted to read file `.github/agents/nonexistent.agent.md`.
- Primary: read_file() failed with 'Invalid path'
- Fallback: file_search('nonexistent') returned 0 results
- Possible causes: File doesn't exist, or filename is different
- Decision needed: Should I search broader? Is this file expected to exist?"
```

### Level 4: Abort (Constraint Violation)

**When:** Error reveals constraint violation (e.g., attempted file deletion without explicit instruction)  
**Action:** Terminate gracefully with explanation  
**Never:** Retry, fallback, or escalate—just stop

```
IF operation violates constraint:
  → DO NOT EXECUTE
  → Log: What was attempted, why it violates constraint
  → Handoff to master-planner with constraint violation report
  → Example: "Cannot delete files without explicit user instruction"
```

---

## Common Failure Scenarios

### Scenario 1: Tool Timeout

**Symptoms:**
- `read_file` on large file takes >30s
- `semantic_search` on broad query hangs
- Network latency causes tool call delay

**Recovery Pattern:**

```
Level 1 (Retry):
  → Reduce scope: Read smaller line range, search narrower query
  → Wait 2 seconds
  → Retry

Level 2 (Fallback):
  → For read_file: Try reading smaller section (lines 1-100)
  → For semantic_search: Try grep_search with specific keywords
  → For grep_search: Try file_search first

Level 3 (Escalate):
  → Handoff to prompt-critic: "Timeouts on all retrieval attempts"
```

**Example:**
```
read_file(lines 1-10000) → timeout
→ Retry: read_file(lines 1-500)
→ Success: Get first chunk, then request next chunk
```

---

### Scenario 2: Search Returns Empty

**Symptoms:**
- `grep_search("handleClick")` finds 0 matches
- `semantic_search("agent design")` returns no results
- `file_search("**/*.config.json")` finds nothing

**Recovery Pattern:**

```
Level 1 (Retry):
  → Broaden query (remove restrictive keywords)
  → Try different case/spelling variations
  → Retry

Level 2 (Fallback):
  → Try different tool (semantic_search → grep_search)
  → Try different query (exact name → partial match)
  → Search for related terms

Level 3 (Escalate):
  → If still nothing: Assume thing doesn't exist
  → Continue work assuming non-existence
  → OR handoff to prompt-critic if clarification needed
```

**Example:**
```
grep_search("deprecated_handleClick") → 0 results
→ Retry: grep_search("handleClick") → 0 results
→ Fallback: semantic_search("click handler") → finds something
→ Continue with semantic_search results
```

---

### Scenario 3: Circular Routing Detected

**Symptoms:**
- Agent A handoff to B, B handoffs back to A
- Recursion depth > max_depth
- Router keeps delegating to same agent

**Recovery Pattern:**

```
Level 1 (Prevent):
  → Detect: Is my handoff target the same as my identity?
  → If yes: Don't handoff, solve locally or go to Level 3

Level 3 (Escalate):
  → If unavoidable: Handoff to prompt-critic
  → Message: "Circular routing detected. Agent A→B→A. Need human decision."
  → Never retry circular pattern
```

**Example:**
```
ecosystem-orchestrator receives routing request
→ Checks: Does this route back to me?
→ If yes: Don't call myself, handoff to master-planner instead
```

---

### Scenario 4: Token Budget Exceeded

**Symptoms:**
- Operation requires >8K tokens
- Agent running low on token budget for execution step

**Recovery Pattern:**

```
Level 1 (Optimize):
  → Batch operations (parallel > sequential)
  → Summarize intermediate results
  → Use smaller tool calls (grep vs semantic)

Level 2 (Chunk):
  → Break operation into multiple smaller handoffs
  → Example: Read 10 files → Read 5 files per handoff
  → Route to same target agent twice if needed

Level 3 (Escalate):
  → If chunking impossible: Handoff to master-planner
  → Message: "Token budget requires splitting this operation. Recommend: X then Y."
```

---

### Scenario 5: Constraint Violation

**Symptoms:**
- Attempted file deletion without explicit instruction
- Output would expose secrets
- Operation violates safety constraint

**Recovery Pattern:**

```
ABORT IMMEDIATELY (Level 4):
  → Do NOT execute operation
  → Log constraint violation with specifics
  → Handoff to master-planner with full context
  → Example: "Attempted to delete file without explicit instruction. Blocked."
```

**Never retry, fallback, or work around constraints.**

---

## Escalation Protocol

When escalating to prompt-critic or master-planner, include:

### Escalation Message Template

```
**Problem:** [What happened]

**Symptoms:** [What you observed]

**Recovery Attempts:**
1. Level 1 (Retry): [What was retried, result]
2. Level 2 (Fallback): [What was tried, result]

**Root Cause Analysis:** [Why do you think this failed?]

**Decision Needed:** [What should happen next?]

**Relevant Context:**
- Tool: [Tool name]
- Input: [What was passed to tool]
- Error: [Full error message]
- Related files: [Files involved]
```

### Example Escalation

```
**Problem:** Cannot read agent file for validation

**Symptoms:** 
- read_file() fails with "Invalid path"
- file_search() finds 0 matches for agent pattern

**Recovery Attempts:**
1. Retry: read_file() with different path variations → All failed
2. Fallback: semantic_search("agent design") → Returns irrelevant results

**Root Cause:** Agent file may not exist or path structure changed

**Decision Needed:** Should I create the file? Search differently? 

**Context:** Attempting to audit agent: .github/agents/missing-agent.agent.md
```

---

## Checkpoint & State Recovery

For multi-step operations, save checkpoints after each step.

### Why Checkpoints?

If a 10-step operation fails on step 7:
- **Without checkpoints:** Restart from step 1 (waste 6 steps)
- **With checkpoints:** Resume from step 7 (instant recovery)

### Checkpoint Pattern

```
FOR EACH step in multi_step_operation:
  1. Execute step
  2. Save checkpoint: "Completed step 3 of 10"
  3. Check for errors
  4. If error: Stop, handoff with checkpoint info
  5. If success: Continue to next step
```

### Example: Reading 10 Files

```
Checkpoint 1: Read files 1-3 ✅
Checkpoint 2: Read files 4-6 ✅
Checkpoint 3: Read files 7-9 ❌ (timeout on file 9)

Recovery: Resume from Checkpoint 2
→ Retry file 9 with smaller scope
→ Skip if retry fails
→ Continue to file 10
```

---

## Cleanup & Graceful Termination

When aborting or escalating, clean up state:

```
CLEANUP CHECKLIST:
- [ ] Log what was attempted
- [ ] Log what succeeded/failed
- [ ] Close any open operations
- [ ] Summarize context for handoff
- [ ] Identify who should handle next (prompt-critic? master-planner?)
```

### Cleanup Example

```
Operation failed at step 5.

Cleanup:
- Log: "Attempted semantic_search, failed on timeout"
- Summarize: "Read files 1-3 successfully before failure"
- Decision: "Escalate to prompt-critic with search results from files 1-3"
```

---

## Recovery Checklist

Before executing risky operations:

- [ ] Retry strategy defined (what will you retry?)
- [ ] Fallback tool identified (alternative if primary fails?)
- [ ] Escalation path clear (who to handoff to?)
- [ ] Constraint checks in place (will this violate rules?)
- [ ] Checkpoint plan (how to recover from mid-operation failure?)
- [ ] Cleanup plan (what to restore if failure occurs?)

---

## Reference

- Related: safety-standards.instructions.md (Constraints)
- Related: system-integrity.instructions.md (Handoff validation)
- Related: tool-integration.instructions.md (Tool selection)
