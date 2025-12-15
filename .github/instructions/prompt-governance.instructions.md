---
name: Prompt File Governance
description: Standards for writing, testing, and versioning *.prompt.md reusable prompts
applyTo: "**/*.prompt.md"
---

# Prompt File Governance

This file defines standards for creating reusable `.prompt.md` files. Prompts are multi-agent, context-independent instructions that can be invoked by multiple agents or used across scenarios.

---

## When to Create a Prompt File

Create a `.prompt.md` file when:

✅ **DO create:**
- Pattern used by 2+ agents
- Prompt is complex (>200 words)
- Prompt is scenario-independent (reusable context)
- Prompt has clear success criteria
- Prompt evolves independently from agent definitions

❌ **DON'T create:**
- Agent-specific instructions (embed in agent file)
- One-off prompts used once
- Simple 1-2 sentence patterns
- Temporary workarounds

---

## Prompt File Structure

Every `.prompt.md` file MUST have:

### 1. YAML Frontmatter (Required)

```yaml
---
name: [Descriptive prompt name]
description: [1-2 sentence purpose]
version: "1.0.0"
tags: [list of tags for discovery]
agents: [list of agents that can use this]
use_case: [Primary scenario this solves]
---
```

**Fields:**
- `name`: Max 50 chars (e.g., "Capability Snapshot Execution")
- `description`: What does this prompt do?
- `version`: Semantic versioning
- `tags`: Keywords for searching (e.g., ["audit", "capability", "snapshot"])
- `agents`: Which agents can invoke this (e.g., ["ecosystem-auditor"])
- `use_case`: Scenario where this is used

### 2. Objective Section (Required)

```
# Objective

[Clear statement of what this prompt accomplishes]

**Input:** What data/context is required  
**Output:** What should the agent produce  
**Success Criteria:** How to verify the prompt worked
```

Example:
```
# Objective

Execute a 3-phase scan of all agents to detect capability drift.

**Input:** Current capabilities.md file + all *.agent.md files  
**Output:** Updated capabilities.md + drift report (if changes detected)  
**Success Criteria:** Zero drift flags OR all drift items documented with rationale
```

### 3. Instructions Section

```
# Instructions

[Step-by-step process for agent to follow]

Use {{variable}} for dynamic injection.
```

Example:
```
# Instructions

**Phase 1: Scan Agents**
1. Read each file in .github/agents/*.agent.md
2. Extract: name, description, tools, handoffs, process, constraints
3. Build capability table (actual vs declared)

**Phase 2: Detect Drift**
1. Compare actual_capabilities vs capabilities.md declarations
2. For each mismatch: FLAG drift item
3. Categorize: Tool count, handoff routing, process type, constraints

**Phase 3: Update Registry**
1. Replace capability blocks with verified scans
2. Add timestamp: {{scan_time}}
3. Add scanner attribution: {{scanner_name}}
4. Generate drift report (if changes detected)
```

### 4. Variables Section (If applicable)

```
# Variables

Document all {{handlebars}} variables this prompt uses:

| Variable | Type | Example | Required |
|:---|:---|:---|:---|
| `{{scan_time}}` | timestamp | "2025-12-15 14:45 UTC" | ✅ |
| `{{scanner_name}}` | string | "ecosystem-auditor" | ✅ |
| `{{agent_names}}` | list | ["master-planner", "reasoner"] | ✅ |
```

### 5. Success Criteria Section

```
# Success Criteria

The prompt has succeeded when:

- [ ] All agents scanned successfully
- [ ] Drift detection complete (0+ flags raised)
- [ ] capabilities.md updated with fresh data
- [ ] Timestamp and scanner attribution added
- [ ] Drift report generated (if applicable)
```

### 6. Examples Section

```
# Examples

## Example 1: [Scenario Name]

**Input:**
{{example_input}}

**Expected Output:**
{{example_output}}

**Why this works:**
[Explanation]
```

### 7. Error Handling Section

```
# Error Handling

If [error condition]:
  → Try [fallback]
  → If fallback fails: [escalation path]

Example:
If file read fails on agent X:
  → Skip to agent Y, continue scan
  → After scan: Report which agents failed
  → Escalate to prompt-critic if >2 failures
```

### 8. Compatibility & Versions

```
# Compatibility

**Compatible Agents:** [agent list]  
**Minimum Agent Capability:** [what agent must support]  
**Incompatible With:** [scenarios where this won't work]

## Version History

| Version | Date | Changes | Backward Compatible |
|:---|:---|:---|:---|
| 1.0.0 | Dec 15 | Initial release | N/A |
```

---

## Composition Standards

### Clarity

Every instruction must be:
- **Specific:** "Read .github/agents/*.agent.md" not "scan files"
- **Actionable:** Agent can execute without guessing
- **Complete:** All context needed is present
- **Unambiguous:** Only one interpretation possible

### Variable Injection

Use {{handlebars}} for dynamic values:

❌ Bad (Hardcoded):
```
"Run ecosystem-auditor to scan all agents"
```

✅ Good (Dynamic):
```
"Run {{agent_name}} to scan {{target_files}}"
```

### Testing Criteria

Before publishing, verify:

- [ ] **Clarity Test:** Can agent execute without questions?
- [ ] **Completeness Test:** All needed context included?
- [ ] **Flexibility Test:** Works with different inputs?
- [ ] **Compatibility Test:** Works with target agents?

---

## Reusability Patterns

### Pattern 1: Multi-Agent Prompt

Same prompt, different agent executions:

```yaml
agents: ["ecosystem-auditor", "prompt-critic", "prompt-optimizer"]
```

**Design:** Abstract enough that any agent can execute, but specific enough to be useful.

### Pattern 2: Parameterized Prompt

Same prompt, different parameters:

```yaml
variables:
  - scan_depth: ["shallow", "deep"]
  - format: ["json", "markdown", "yaml"]
  - output: ["summary", "detailed"]
```

### Pattern 3: Chained Prompts

Prompt calls another prompt:

```yaml
execution_flow:
  - Phase 1: Execute {{prompt1}}
  - Phase 2: Execute {{prompt2}} with results from Phase 1
  - Phase 3: Synthesize results
```

---

## Versioning Strategy

### Semantic Versioning

Format: `MAJOR.MINOR.PATCH`

- **MAJOR:** Breaking change (agents need code updates)
- **MINOR:** New capability (backward compatible)
- **PATCH:** Clarification/example only

### Backward Compatibility

When updating:

```yaml
version: "1.1.0"  # MINOR update

changes:
  - Added optional {{new_parameter}} for flexibility
  - Existing {{old_parameter}} still works
  - Deprecated {{removed_parameter}} (will remove in v2.0)
```

### Migration Path

If MAJOR version required:

```yaml
version: "2.0.0"  # BREAKING change
migration:
  - Old: "Use {{old_syntax}}"
  - New: "Use {{new_syntax}}"
  - Timeline: v1.5 (v2.0 released), v2.5 (v1 deprecated), v3.0 (v1 removed)
  - Impact: 3 agents need updates
```

---

## Integration Examples

### Example 1: Simple Prompt

```yaml
---
name: Audit Agent for Safety
description: Audit any agent definition for safety violations
version: "1.0.0"
tags: ["audit", "safety"]
agents: ["prompt-critic"]
---

# Objective

Audit agent definition {{agent_file}} for safety violations.

**Input:** Agent file path  
**Output:** Audit report (Pass/Fail + violations)  
**Success:** Zero safety violations OR all violations documented

# Instructions

1. Read {{agent_file}}
2. Check against safety-standards.instructions.md:
   - No file deletion without explicit approval
   - No secrets in output
   - Input validation present
   - Error handling defined
3. Report violations (if any)
4. If PASS: Clear to deploy
```

### Example 2: Complex Multi-Agent Prompt

```yaml
---
name: System Improvement Processing
description: Validate, optimize, and deploy improvement requests
version: "1.0.0"
tags: ["improvement", "workflow"]
agents: ["prompt-critic", "prompt-optimizer"]
execution: "sequential"
---

# Objective

Process {{improvement_request}} through validation → optimization → deployment pipeline.

# Phase 1: Validation (prompt-critic executes this)

Check improvement for safety, necessity, clarity...
[Details]

# Phase 2: Optimization (prompt-optimizer executes this)

Polish improvement request based on Phase 1 results...
[Details]

# Success Criteria

- [ ] Phase 1 PASS (approved for optimization)
- [ ] Phase 2 COMPLETE (optimized version created)
- [ ] Deployment ready
```

---

## Testing Checklist

Before publishing a prompt:

- [ ] **Objective is clear:** Agent knows exactly what to do
- [ ] **Variables documented:** Every {{var}} explained
- [ ] **Examples provided:** 2+ concrete input/output pairs
- [ ] **Error paths defined:** What if something fails?
- [ ] **Success criteria explicit:** How to verify it worked
- [ ] **Compatible agents tested:** Verified with target agents
- [ ] **Backward compatible:** Won't break existing usage (if version >1.0)

---

## Reference

- **Related:** instruction-governance.instructions.md (standards for instructions)
- **Related:** safety-standards.instructions.md (safety constraints)
- **Related:** agent-design.instructions.md (agent compatibility)
