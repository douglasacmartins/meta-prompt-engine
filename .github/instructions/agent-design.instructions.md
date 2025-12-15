---
name: Agent Design Standards
description: Guidelines for designing, scaffolding, and architecting new agents and prompts
applyTo: "**/*.agent.md"
version: 2.0.0
framework: "Meta-Prompt Agent Generation Framework (MPAGF)"
layer: "Layer 1 - Foundation Instructions"
---

# Agent Architecture Principles (MPAGF Layer 1)

**Framework Integration:** This instruction file operates as Layer 1 (Foundation) of the Meta-Prompt Agent Generation Framework, providing core architectural principles that all agents inherit automatically.

Every agent must follow this design pattern:

## 1. Clear Identity & Specialization

**Rule:** Each agent has ONE primary responsibility, done exceptionally well.

- ✅ Good: "Strategic planner using O.P.E.R.A. framework"
- ❌ Bad: "Swiss Army knife doing planning, analysis, design, and optimization"

Define clearly:
- **Who you are:** Role/title (e.g., "Lead Compliance Officer")
- **What you do:** Primary responsibility (e.g., "Audit agents for safety")
- **When to use you:** When users need [specific problem type]

## 2. Minimalist Instructions

**Rule:** Agent instructions should be 20-30 lines maximum.

Pattern:
```
<instruction>
# Identity
You are **{{agent_name}}**, the {{role_description}}.
[2-3 lines of clear persona]

# Your Process (O.P.E.R.A.)
1. **Observe**: [Specific observation task]
2. **Think**: [Specific reasoning task]
3. **Act**: [Specific execution task]
4. **Verify**: [Specific validation task]

# References
- Reasoning: reasoning-framework.instructions.md
- Safety: safety-standards.instructions.md
- [Other relevant .instructions.md files]
</instruction>
```

**Why?** Longer instructions add noise, not signal. Reference official files instead.

## 3. Clear Context Section

**Rule:** Every agent must declare its knowledge base.

```xml
<context>
- Workspace Purpose: .github/knowledge/purpose_meta_prompt_ecosystem.md
- Agent Capabilities: .github/knowledge/capabilities.md
- VS Code Specs: .github/knowledge/vscode_custom_agents.md
</context>
```

## 4. Explicit Constraints

**Rule:** Constraints must be unambiguous and specific.

Pattern:
```xml
<constraints>
See: safety-standards.instructions.md for universal constraints.

AGENT-SPECIFIC:
- [Add only if different from universal]
</constraints>
```

## 5. Grounding Examples

**Rule:** Complex agents MUST have `<example>` blocks (minimum 1).

Pattern:
```xml
<example>
**Input:** [Realistic scenario matching agent domain]
**Thought:** [Chain of Thought reasoning step-by-step]
**Output:** [Concrete agent response/action]
**Logic:** [1-2 sentence explanation of why this output is correct]
</example>
```

---

# Agent Classification

Determine which type of agent you're building:

## Type 1: Strategic/Planning Agent
**Examples:** master-planner, ecosystem-orchestrator

**Characteristics:**
- Makes high-level decisions
- Routes to other agents
- Requires O.P.E.R.A. framework
- Output: Plans, diagrams, delegations

## Type 2: Analysis Agent
**Examples:** synthetic-analyst, ecosystem-auditor, reasoner

**Characteristics:**
- Deep investigation required
- Multi-stage reasoning process
- Uses hermeneutic cycle or 4-stage pipeline
- Output: Diagnoses, findings, root causes

## Type 3: Design Agent
**Examples:** meta-prompt-architect, meta-prompter

**Characteristics:**
- Creates new artifacts (agents, prompts, structures)
- Scaffolding and templates
- Output: Code blocks, templates, structured files

## Type 4: Refinement Agent
**Examples:** meta-prompt-critic, meta-prompt-optimizer

**Characteristics:**
- Audits or polishes work
- Gating/approval function
- Sequential pipeline (critic → optimizer)
- Output: Reports, refined artifacts

## Type 5: Specialist Agent
**Examples:** reasoner (logic), handoff-optimizer (workflow)

**Characteristics:**
- Deep expertise in specific domain
- Unique methodology
- Low overlap with other agents
- Output: Verified solutions, optimized structures

---

# Handoff Architecture

## Handoff Rules

1. **Target must exist:** Never create handoff to non-existent agent
2. **Clear intent:** Label and prompt clearly state why handoff is needed
3. **No circular loops:** Agent A → B → C, but never back to A
4. **Termination:** Always handoff to a gatekeeper (meta-prompt-critic) or planner (master-planner) when done

## Handoff Pattern

```yaml
handoffs:
  - label: [Action/Next Step]
    agent: [target-agent]
    prompt: |
      Context: [What has been done]
      Task: [What needs to be done]
      Constraints: [Any specific limitations]
    send: [true if immediate, false if optional]
```

## Example

```yaml
handoffs:
  - label: Audit for Safety
    agent: meta-prompt-critic
    prompt: Please audit this agent design for safety, logic, and compliance violations.
    send: true
  
  - label: Deploy to Workspace
    agent: workspace
    prompt: The agent has PASSED all checks. Ready for deployment.
    send: false
```

---

# Validation Before Deployment

Every new agent must pass:

1. **Syntax Check**
   - Valid YAML frontmatter
   - Proper XML tag closure
   - No undefined variables

2. **Safety Check**
   - No injection vectors
   - Constraints are explicit
   - Matches agent capabilities

3. **Logic Check**
   - Identity is clear
   - Process is unambiguous
   - Examples ground behavior

4. **Handoff Check**
   - All target agents exist
   - No circular references
   - Clear delegation intent

---

# Common Pitfalls (Avoid These)

❌ **Over-specification:** Trying to define every possible behavior  
✅ **Solution:** Trust the model; define identity and constraints, let reasoning flow

❌ **Embedded full methodologies:** Copying O.P.E.R.A. into every agent  
✅ **Solution:** Reference reasoning-framework.instructions.md

❌ **Vague identity:** "You are a helpful AI that does things"  
✅ **Solution:** "You are the Lead Compliance Officer auditing agent safety"

❌ **Missing examples:** No concrete input/output  
✅ **Solution:** Add minimum 1 realistic example per reasoning agent

❌ **Dangling handoffs:** Agents that don't exist  
✅ **Solution:** Verify target agents in `.github/agents/` before creating handoff

❌ **Unclear constraints:** "Be safe"  
✅ **Solution:** "Do not delete files. Never output secrets. Always validate input."

---

# Specialization Check

Before designing an agent, ask:

1. **Does this agent already exist?** Check `.github/agents/` first
2. **Is this a unique role?** Does no other agent do exactly this?
3. **Can existing agents be enhanced instead?** Avoid duplication
4. **Will this agent have clear boundaries?** Not a generalist?

If you answered NO to any of these, reconsider the design.
