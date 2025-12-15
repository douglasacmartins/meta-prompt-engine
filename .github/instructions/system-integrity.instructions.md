---
name: System Integrity Standards
description: DRY principle enforcement, handoff validation, and ecosystem coherence
applyTo: "**/*.agent.md"
version: 2.0.0
framework: "Meta-Prompt Agent Generation Framework (MPAGF)"
layer: "Layer 1 - Foundation Instructions"
---

# DRY Principle (MPAGF Layer 1 - Don't Repeat Yourself)

**The Golden Rule:** No constraint, context, pattern, or methodology should be duplicated across agents.

## Enforcement

When you identify duplication:

```
IF constraint/pattern appears in 2+ places:
  → Move to appropriate .instructions.md file (official location)
  → Update all occurrences to reference the file
  → Verify zero duplication remains
  RESULT: Single source of truth, automatic updates for all agents
```

## Why DRY Matters

**Without DRY:**
- 1 constraint change = edit 5+ agent files
- Risk of inconsistency and divergence
- Cognitive load: 120+ duplicated lines
- Maintenance burden: 15+ minutes per change

**With DRY:**
- 1 constraint change = edit 1 .instructions.md file
- Automatic consistency across system
- Cognitive clarity: zero duplication
- Maintenance: 30 seconds per change

## Shared Instruction Files (Source of Truth)

All agents reference these files instead of duplicating:

```
.github/instructions/
├── reasoning-framework.instructions.md    (O.P.E.R.A., patterns)
├── safety-standards.instructions.md       (Constraints, audit rules)
├── agent-design.instructions.md           (Agent creation standards)
├── prompt-engineering.instructions.md     (CoT, examples, optimization)
└── system-integrity.instructions.md       (This file - DRY, handoffs)
```

If you need a capability NOT in these files:
1. Check if it already exists (search the .instructions.md files)
2. If missing: Add it to the appropriate file
3. Document with clear examples
4. Reference from your agent

---

# Handoff Validation (Mandatory)

**Rule:** Every handoff must point to an existing agent. No exceptions.

## Pre-Handoff Checklist

Before creating a handoff:

- [ ] Target agent exists in `.github/agents/`
- [ ] Agent file name matches handoff target
- [ ] Agent's identity is clear and well-defined
- [ ] Handoff purpose is explicit (label + prompt)
- [ ] No circular dependencies (A→B→C, never back to A)

## Forbidden Patterns

❌ **Circular Routing:**
```yaml
A → B → C → A  # Creates infinite loop
```

✅ **Proper Routing:**
```yaml
A → B → C → prompt-critic (gatekeeper)
```

❌ **Dangling Handoff:**
```yaml
handoffs:
  - agent: non-existent-agent  # Breaks workflow
```

✅ **Verified Handoff:**
```yaml
handoffs:
  - agent: prompt-critic  # Exists in .github/agents/prompt-critic.agent.md
```

## Handoff Template

```yaml
handoffs:
  - label: [Specific action/next step]
    agent: [verified-target-agent]
    prompt: |
      Context about what was done: [summary]
      What to do next: [clear instructions]
      What to look for: [specific criteria]
    send: [true if immediate handoff, false if optional]
```

## Example

```yaml
handoffs:
  - label: Audit for Safety
    agent: prompt-critic
    prompt: |
      I have designed a new ecosystem-optimizer agent.
      Please audit it for safety violations, logic gaps, and compliance issues.
      Report using the Constitutional Compliance checklist from safety-standards.instructions.md.
    send: true
```

---

# Recursion Guards

**Rule:** Prevent infinite loops in agent routing.

## Max Depth Rule

```
ecosystem-orchestrator should NOT call itself
  OR should have max_depth = 1 (only one self-reference allowed)
```

## Self-Reference Check

```
IF agent wants to call itself:
  → Is this truly necessary?
  → Can another agent handle this?
  → If yes: Change handoff to different agent
  → If no: Add max_depth=1 + termination rule
```

## Example: Orchestrator Safety

```
Recursion Prevention (ecosystem-orchestrator):
  1. Max Depth: Do not chain orchestrator calls
  2. Self-Reference Check: If routing request mentions "orchestrator", delegate to master-planner
  3. Termination Rule: If request_depth > max_depth, route to reasoner for verification
```

---

# Ecosystem Coherence Metrics

Measure system health with these metrics:

## Duplication Metrics

```
DRY Violation Score:
  = (Duplicated Lines) / (Total Instruction Lines) × 100
  Target: < 5%
  
Constraint Duplication:
  = (Constraints in 2+ files) / (Total Constraints) × 100
  Target: 0%
  
Pattern Reuse Rate:
  = (Shared patterns used) / (Total patterns) × 100
  Target: 90%+
```

## Specialization Metrics

```
Agent Overlap Score:
  = (Agents with >50% overlapping responsibility) / (Total Agents)
  Target: 0%
  
Unique Capability Per Agent:
  = (Agent-specific logic lines) / (Total lines per agent)
  Target: 60%+
```

## Architectural Metrics

```
Handoff Validity:
  = (Valid handoffs) / (Total handoffs) × 100
  Target: 100%
  
Recursion Violations:
  = (Circular dependencies found)
  Target: 0
  
Instruction Coverage:
  = (Agents referencing .instructions.md) / (Total agents) × 100
  Target: 100%
```

---

# Audit Workflow

When auditing ecosystem integrity:

## Step 1: Duplication Scan
```
Search for:
  - Duplicated O.P.E.R.A. definitions
  - Duplicated constraint lists
  - Duplicated reasoning patterns
  - Duplicated context blocks

Report: List of duplications with agent references
Action: Move to appropriate .instructions.md file
```

## Step 2: Handoff Validation
```
For each agent:
  - Verify all handoff targets exist
  - Check for circular dependencies
  - Confirm clear delegation intent
  - Test routing logic

Report: Valid/invalid handoff summary
Action: Fix dangling or circular handoffs
```

## Step 3: Specialization Review
```
For each agent:
  - Confirm unique primary responsibility
  - Identify overlaps with other agents
  - Assess consolidation opportunities
  - Measure unique logic percentage

Report: Agent specialization matrix
Action: Recommend consolidations if >50% overlap
```

## Step 4: Instruction Compliance
```
For each agent:
  - Verify references to .instructions.md files
  - Check for missing context sections
  - Confirm constraints are referenced or justified
  - Validate example blocks exist

Report: Compliance checklist per agent
Action: Update agents to match official structure
```

---

# Deployment Checklist

Before deploying changes to ecosystem:

- [ ] DRY compliance score > 95%
- [ ] Zero duplicated constraints
- [ ] All handoffs verified (target agents exist)
- [ ] No circular dependencies
- [ ] All agents reference official .instructions.md files
- [ ] No agent has >50% overlap with another
- [ ] All example blocks are concrete and realistic
- [ ] XML structure valid in all files
- [ ] YAML frontmatter complete in all agents
- [ ] No hardcoded values (use {{handlebars}})

---

# Continuous Integrity

**After each change:**

1. Run DRY audit (check duplication)
2. Validate handoffs (check targets exist)
3. Measure metrics (track improvement)
4. Document changes (update audit trail)

**Monthly:**

1. Review specialization (are agents still distinct?)
2. Audit instruction coverage (are agents using .instructions.md?)
3. Check for new duplication patterns
4. Consolidate if overlap detected

---

# System Improvement Protocol

When the ecosystem needs enhancement:

1. **Diagnose Gap:**
   ```
   "Agents cannot {{action}}.
    Current capabilities: {{what_they_can_do}}.
    Missing: {{what's needed}}."
   ```

2. **Route Improvement Request:**
   - Send to `@prompt-critic` (validates safety/necessity)
   - If approved → `@prompt-optimizer` (polish)
   - Deploy to `.github/instructions/` or `.github/agents/`

3. **Track in Capabilities Registry:**
   - Document in `.github/knowledge/capabilities.md`
   - Add to improvements_log with date and impact

4. **Measure Impact:**
   - Did duplication decrease?
   - Did agents become more specialized?
   - Did system coherence improve?

---

# Performance & Efficiency Standards

Agents must operate efficiently within resource constraints. This section defines performance targets and optimization patterns.

---

## Token Budget Guidelines

Each agent type has a target token budget for typical operations. Plan your tool usage and output size accordingly:

| Agent Type | Context Budget | Instruction | Output Limit | Total Target |
|:---|---:|---:|---:|---:|
| **Planning Agents** (master-planner, orchestrator) | 2K | 1K | 5K | 8-10K |
| **Analysis Agents** (reasoner, synthetic-analyst) | 2K | 1K | 4K | 6-8K |
| **Design Agents** (prompt-architect, meta-prompter) | 1.5K | 0.8K | 3.5K | 5-7K |
| **Refinement Agents** (critic, optimizer) | 1.5K | 0.8K | 3K | 4-6K |
| **Utility Agents** (handoff-optimizer, auditor) | 1.5K | 0.8K | 2.5K | 4-5K |

**How to stay within budget:**

- Batch independent tool calls (parallel execution saves round-trips)
- Read files once with large ranges (not multiple small reads)
- Summarize intermediate results before handoff
- Use grep_search instead of semantic_search when pattern matching is enough
- Limit output to essential information only

---

## Parallel Execution Patterns

When tool calls don't depend on each other, execute them in parallel (same function_calls block).

**Performance Impact:**
- Sequential (5 read_file calls): ~5 seconds
- Parallel (5 read_file calls): ~1 second
- **Savings: 4 seconds per operation**

**When to batch:**
- Multiple `read_file` calls to different files ✅
- Multiple `grep_search` calls to different queries ✅
- `semantic_search` + `list_dir` together ✅

**When NOT to batch:**
- Tool A's output feeds into Tool B (dependent) ❌
- Result of search determines next read ❌
- Edit operation depends on read contents ❌

**Pattern:**
```
<function_calls>
  <invoke name="tool1">...</invoke>
  <invoke name="tool2">...</invoke>
  <invoke name="tool3">...</invoke>
</function_calls>
```

Result: 3x faster than sequential.

---

## Query Optimization

Choose the right tool and optimize queries to minimize cost:

### semantic_search (Expensive)
- Cost: Slowest, highest token consumption
- Use: Understanding context, finding related concepts
- Optimization: Combine multiple queries into one broad search

❌ Bad: `semantic_search("agent design")`, `semantic_search("specialization")`, `semantic_search("classification")`  
✅ Good: `semantic_search("agent design, specialization, classification")`

### grep_search (Medium Cost)
- Cost: Fast, pattern-based
- Use: Finding specific keywords, function names
- Optimization: Use regex to narrow results upfront

❌ Bad: `grep_search("function")`  
✅ Good: `grep_search("^export function", isRegexp=true)`

### file_search (Fast)
- Cost: Instant, glob-based
- Use: Finding files by name/pattern
- Optimization: Use specific patterns, not wildcards

❌ Bad: `file_search("*")`  
✅ Good: `file_search("**/*.agent.md")`

### read_file (Balanced)
- Cost: Fast, but token cost = file size
- Use: When you need exact contents
- Optimization: Read large sections once, not many small reads

❌ Bad: Read lines 1-50, then 50-100, then 100-150  
✅ Good: Read lines 1-150 in single call

---

## Output Size Management

Manage output size to stay within token budgets:

**Handoff Output:**
- Keep handoff context to essential information only
- Summarize findings before routing to next agent
- Example: "Found 12 matches, 3 are critical, here they are..."

**Large Results Handling:**
- If output exceeds 3K tokens, summarize instead of listing all
- Offer to provide details in follow-up if needed
- Chunk large operations: Split 100 reads into 20 reads per handoff

**Example Summarization:**
```
❌ Bad (3K tokens, excessive detail):
"Found these 50 agents with detailed info on each..."

✅ Good (200 tokens, actionable summary):
"Found 50 agents. 3 require immediate fixes: A (xyz), B (abc), C (def). 
Details available on request."
```

---

## Performance Checklist

Before executing agent operations:

- [ ] Tool calls batched where independent (parallel not sequential)
- [ ] Query patterns optimized (right tool for job)
- [ ] Output summarized before handoff (not overwhelming detail)
- [ ] Token budget estimated pre-execution
- [ ] Large operations chunked if exceeding limits
- [ ] Parallel read_file used instead of sequential reads
- [ ] Fallback tools identified if primary is expensive

---

# Reference

- **Reasoning:** reasoning-framework.instructions.md
- **Safety:** safety-standards.instructions.md
- **Design:** agent-design.instructions.md
- **Optimization:** prompt-engineering.instructions.md
- **Tool Usage:** tool-integration.instructions.md
- **Error Handling:** error-recovery.instructions.md
- **Capabilities:** .github/knowledge/capabilities.md
