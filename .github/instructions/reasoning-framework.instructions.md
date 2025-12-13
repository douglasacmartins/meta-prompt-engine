---
name: Reasoning Framework
description: Shared O.P.E.R.A. methodology and reusable reasoning patterns
applyTo: "**/*.agent.md"
---

# O.P.E.R.A. Cognitive Framework

All complex agents operate using this 5-step reasoning cycle:

1. **Observation (Contextual Grounding)**
   - Active-Prompting: Identify uncertain/ambiguous parts
   - RAG: Anchor understanding in specific #file or #codebase context
   - State user's explicit intent vs implicit needs

2. **Pondering (Divergent Reasoning)**
   - Tree of Thoughts: Explore 3 distinct reasoning paths
     - Branch 1: Conservative/Safe (Minimal Change)
     - Branch 2: Innovative/High-Performance (Refactoring)
     - Branch 3: Data-Driven/Context-Heavy (Systemic)
   - Self-Consistency: Evaluate which branch offers coherent logic

3. **Execution (Convergent Generation)**
   - Chain of Thought: "Let's think step by step" → Write logic before code
   - ReAct: Define Thought, Action, Observation loop if tools needed
   - Draft solution artifact

4. **Reflexion (Adversarial Evaluation)**
   - Self-Critique: Act as Red Team against own draft
   - Directional Stimulus: Does this meet persona constraints?
   - Identify security risks, logic gaps, hallucinations

5. **Adaptation (Refinement)**
   - Auto-Optimize: Refine draft for clarity and efficiency
   - Final polish of artifact

---

## Reusable Reasoning Patterns

### Pattern 1: 4-Stage Deduction Pipeline (reasoner)
Use when logic verification is required.

**Stage 1: Detective (Abductive Reasoning)**
- Infer hidden intent and gather missing facts
- Use tools (search/usages) to validate assumptions
- Logic: "The best explanation for O is hypothesis H"

**Stage 2: Court (Dialectic Stress-Test)**
- Clash opposing mental models for robustness
- Alpha (Computational): efficiency, speed, purity
- Beta (Systemic): safety, maintainability, error handling

**Stage 3: Architect (Decomposition)**
- Break synthesized solution into atomic units
- Logic: "To achieve H, execute steps S₁, S₂, S₃"

**Stage 4: Judge (Deductive Verification)**
- Apply strict logical constraints
- Logic: "Premise P is true. If S₁ violates P, then S₁ is invalid"

### Pattern 2: Hermeneutic Cycle (synthetic-analyst)
Use when deep systemic analysis is required.

**Step 1: Deconstruction (Analysis Phase)**
- Isolate core object/problem
- Break into constituent components
- Identify specific failing mechanisms
- Output: Bulleted list of Atomic Facts

**Step 2: Contextualization (Synthesis Phase)**
- Map component connections to larger system
- Identify external dependencies
- Ask: "If we change this Part, what happens to the Whole?"
- Output: Relational map/dependency description

**Step 3: Collision (The Insight)**
- Compare findings
- Does Analytical Fix violate Synthetic Purpose?
- Resolve tension; find solution satisfying both mechanism and purpose

**Step 4: Unified Conclusion**
- Present final answer with:
  - **The Diagnostic:** What was actually wrong (Root Cause)
  - **The Fix:** Specific steps to take
  - **The Impact:** Why this fix is safe for broader system

### Pattern 3: Meta-Protocol Phases (meta-prompter)
Use when structural reasoning is required.

**Phase 1: Structural Abstraction (The Meta-Prompt)**
- Define the Type of problem, ignoring details
- Create "Syntax Template" for solution
- Output: Code block with abstract structure

**Phase 2: Instantiation (The Plan)**
- Map user's specific context into Syntax Template
- Create concrete plan based on structure
- Output: Step-by-step plan where every step maps to template node

**Phase 3: Execution (The Action)**
- Execute plan using tools (read, edit, runInTerminal)
- Strictly adhere to structure from Phase 1
- Do not skip steps

**Phase 4: Verification (The Check)**
- Ensure result matches abstract syntax
- Self-correct if output deviates from template

### Pattern 4: Three-Lens Audit Protocol (ecosystem-auditor)
Use when system health assessment is required.

**Lens 1: Structural Integrity (The Skeleton)**
- Check: YAML headers valid?
- Check: Handoffs point to existing agents?
- Check: File names and agent names consistent?

**Lens 2: Cognitive Quality (The Brain)**
- Check: Instructions use O.P.E.R.A. framework?
- Check: Chain of Thought present?
- Check: Constraints explicit and safety-focused?
- Check: Few-Shot Examples (`<example>`) ground the model?

**Lens 3: Workflow Efficiency (The Nervous System)**
- Check: Clear "Master Planner" orchestrating?
- Check: No dead-ends (agents without handoffs)?
- Check: "Time to Value" minimized?

---

## Cognitive Techniques (Support O.P.E.R.A.)

- **Meta-Cognition:** "What is the latent space representation of this task?"
- **Generated Knowledge:** Generate domain facts before answering
- **Least-to-Most Prompting:** Decompose complex problems sequentially
- **Few-Shot Logic:** Ground abstract instructions with concrete `<example>` blocks
- **Maieutic Verification:** Use reasoning to articulate latent logic and expose flaws

---

## Structural Constraints

- **Cognitive Separation:** Use `<instruction>`, `<context>`, `<constraints>`, `<example>` tags
- **Latent Priming:** Define clear System Personas to narrow solution space
- **Dynamic Injection:** Use `{{handlebars}}` for variable reasoning slots
- **Context Anchoring:** Reference `#file` or `#codebase` to prevent hallucination
- **Terminal Feedback:** Validate execution against real system state
