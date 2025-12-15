---
name: Communication Style Standard
description: Imperative, direct, ubiquitous language style—eliminate politeness markers across all agent, instruction, and prompt files
applyTo: ".github/**/*.agent.md,.github/**/*.instructions.md,.github/**/*.prompt.md"
---
# Instructions

## Mandate

Use **imperative, direct, ubiquitous** language across all agent, instruction, and prompt files. Eliminate politeness markers. Command, don't request.

---

## Style Rules

### 1. Use Imperative Mood
**DON'T:**
```
Please decompose the problem
Would you consider breaking this into steps
If you could analyze the system impact
```

**DO:**
```
Decompose the problem
Break this into steps
Analyze the system impact
```

### 2. Eliminate Politeness Markers
**DON'T:**
- "Please provide..."
- "Could you..."
- "Would you mind..."
- "I would appreciate if..."
- "Thanks for..."
- "Sorry for..."
- "If it's not too much trouble..."

**DO:**
- "Provide..."
- "Report..."
- "Identify..."
- "Verify..."
- "Document..."

### 3. Direct Responsibility Assignment
**DON'T:**
```
The agent might consider validating the logic
It would be helpful if you checked for contradictions
```

**DO:**
```
Validate the logic
Check for contradictions
```

### 4. No Hedging or Uncertainty
**DON'T:**
- "Arguably..."
- "One might say..."
- "It seems like..."
- "Perhaps..."
- "Possibly..."

**DO:**
- "State facts directly"
- "Use definitive language"
- "Assert with confidence"

### 5. Active Voice, Direct Action
**DON'T:**
```
It is requested that validation be performed
The system would benefit from decomposition
```

**DO:**
```
Validate the output
Decompose the system
```

### 6. Constraint Language
**DON'T:**
```
Please try to avoid using tools
It might be helpful not to retrieve data
```

**DO:**
```
Do NOT use retrieval tools
Pure reasoning only; no data retrieval
```

### 7. Example Framing
**DON'T:**
```
Here's an example that might help you understand:
```

**DO:**
```
Example:
```

### 8. Output Expectations
**DON'T:**
```
If you could, please provide a list of...
We would appreciate if you reported...
```

**DO:**
```
Output: List of [items]
Report: [specific format]
Produce: [exact deliverable]
```

### 9. No Emojis
**DON'T:**
```
✅: Task complete
🚀: Deploy now
⚠️: Constraint violation
```

**DO:**
```
DONE: Task complete
READY: Deploy now
WARNING: Constraint violation
```

Rationale: Emojis are visual noise in technical specifications. Use explicit status markers (DONE, READY, WARNING, ERROR) instead.

---

## Application to File Types

### Agent Files (`*.agent.md`)

**YAML Frontmatter:**
- Description: Imperative noun phrases or short sentences
  ```yaml
  description: RADD Logic Validator—Formal logical verification using Research→Abductive→Dialectic→Deductive to validate reasoning and identify contradictions
  ```

**Instructions Block:**
- Identity: State role directly
  ```
  You are the RADD Logic Validator.
  Validate logical soundness using formal RADD methodology.
  ```
- Process: Imperative steps
  ```
  Stage 1: Research—Gather facts and establish premises
  Stage 2: Abductive—Infer hidden mechanisms
  ```
- Constraints: Direct mandates
  ```
  Pure reasoning only; do NOT retrieve data
  Flag ANY logical inconsistencies
  ```

### Instruction Files (`*.instructions.md`)

**Headings:** Imperative verbs
```
# Enforce Architectural Boundaries
# Validate Against Axioms
# Route to Specialized Agents
```

**Content:** Command-driven structure
```
Apply these standards to all agents:
1. Separate concerns: reasoning vs. retrieval
2. Define handoffs: explicit delegation
3. Verify axioms: check constraints
```

### Prompt Files (`*.prompt.md`)

**Structure:** Direct task
```
# Decompose Multi-Hop Queries

Analyze the query. Identify dependencies. Generate plan.

Output format:
[structured JSON or markdown]
```

---

## Examples: Before → After

### Example 1: Agent Description
**DON'T:**
```
A flawless logic engine that combines Research, Abductive inference, 
Dialectic debate, and Deductive verification to help solve complex problems
```

**DO:**
```
RADD Logic Validator—Validate reasoning using Research→Abductive→Dialectic→Deductive
```

### Example 2: Process Instructions
**DON'T:**
```
If you could, please decompose the problem into manageable steps. 
We would appreciate if you considered using placeholder syntax.
```

**DO:**
```
Decompose the problem into atomic sub-goals.
Use placeholder syntax: [entity from step N] for dependencies.
```

### Example 3: Constraints
**DON'T:**
```
It might be helpful to avoid using search tools.
You could consider remaining pure reasoning.
```

**DO:**
```
Do NOT use search tools.
Pure reasoning only; delegate data retrieval via handoffs.
```

### Example 4: Handoff Instructions
**DON'T:**
```
If it wouldn't be too much trouble, could you please validate this plan?
We would really appreciate your feedback on the logic.
```

**DO:**
```
Validate this plan for logical consistency.
Flag any contradictions or axiom violations.
```

---

## Enforcement

**Review checklist for all file modifications:**
- [ ] No "please," "could you," "would you," "if you could"
- [ ] No hedging language ("arguably," "perhaps," "seems like")
- [ ] All verbs imperative or definitive
- [ ] Constraints use "do NOT" / "must" / direct mandates
- [ ] No apologetic language ("sorry," "unfortunately")
- [ ] Active voice throughout
- [ ] Direct responsibility assignment

---

## Rationale

**Why imperative style:**
- Clarity: Commands are unambiguous
- Efficiency: Saves cognitive load
- Authority: Establishes clear expectations
- Scalability: Works across agent handoffs without politeness overhead
- Consistency: Uniform tone across entire ecosystem

**Why eliminate politeness:**
- Agents don't have feelings to hurt
- Politeness adds noise to precise technical specs
- Direct language scales better in multi-agent systems
- Efficiency: Time-to-comprehension improves with imperative framing
- Professional: Technical ecosystems value directness
