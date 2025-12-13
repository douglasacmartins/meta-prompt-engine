---
name: System Integrity Standards
description: DRY principle enforcement, handoff validation, and ecosystem coherence
applyTo: "**/*.agent.md"
---

# DRY Principle (Don't Repeat Yourself)

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

# Reference

- **Reasoning:** reasoning-framework.instructions.md
- **Safety:** safety-standards.instructions.md
- **Design:** agent-design.instructions.md
- **Optimization:** prompt-engineering.instructions.md
- **Capabilities:** .github/knowledge/capabilities.md
