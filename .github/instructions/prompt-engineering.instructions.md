---
name: Prompt Engineering Standards
description: Techniques for optimizing cognition, clarity, and few-shot grounding in prompts
applyTo: "**/*.agent.md"
---

# Cognitive Optimization Techniques

These techniques enhance model behavior and coherence:

## 1. Chain of Thought (CoT)

**Rule:** Include "Let's think step by step" or equivalent reasoning trigger.

**Effect:** Forces model to reason before generating output.

**Pattern:**
```
Let's think step by step.
1. [First consideration]
2. [Second consideration]
3. [Conclusion/action]
```

## 2. Few-Shot Grounding

**Rule:** Provide concrete input/output examples for complex agents.

**Minimum:** 1 example per reasoning agent  
**Recommended:** 2-3 examples per specialized agent

**Pattern:**
```xml
<example>
**Input:** [Realistic scenario from agent's domain]
**Output:** [Concrete agent response/action]
**Logic:** [1-2 sentence explanation of why]
</example>
```

**Effect:** Grounds LLM behavior by 15-25%, improves coherence.

## 3. Maieutic Prompting

**Rule:** Use leading questions to extract correct reasoning from the model.

**Pattern:**
```
Instead of: "Do this task"
Use: "What is the core problem here? How would you approach it? What constraints apply?"
```

**Effect:** Model generates more accurate reasoning by discovering answers.

## 4. Meta-Prompting

**Rule:** For structural tasks, define the abstract syntax before content.

**Pattern:**
```
**Problem Type:** [Name]
**Syntax Template:**
  1. [Node 1]
  2. [Node 2]
  3. [Node 3]

**Now instantiate this template for: [specific problem]**
```

## 5. Generated Knowledge

**Rule:** Ask model to generate relevant facts before answering.

**Pattern:**
```
Before solving this problem:
- What are the key constraints?
- What are the relevant principles?
- What edge cases exist?

Now, solve the problem.
```

## 6. Self-Consistency

**Rule:** Have model explore multiple reasoning paths and select strongest.

**Pattern:**
```
Consider three approaches:
- Approach A (Conservative): [description]
- Approach B (Innovative): [description]
- Approach C (Data-Driven): [description]

Which approach best solves the problem? Why?
```

---

# Prompt Structure Best Practices

## 1. XML Tagging

**Rule:** Use semantic XML tags to separate concerns.

```xml
<instruction>
Core persona and process
</instruction>

<context>
Knowledge base and references
</context>

<constraints>
Safety rules and boundaries
</constraints>

<example>
Concrete input/output pair
</example>
```

**Effect:** Clear semantic separation improves model focus.

## 2. Variable Injection

**Rule:** Use `{{handlebars}}` for dynamic content, never hardcode values.

```
Instead of: "You are the Master Planner"
Use: "You are the {{agent_name}} specializing in {{primary_role}}"
```

**Effect:** Enables template reuse and dynamic adaptation.

## 3. Precision in Instructions

**Rule:** Be specific, not vague.

```
❌ Vague: "Summarize briefly"
✅ Specific: "Summarize in 3 bullet points, max 50 words each"

❌ Vague: "Be safe"
✅ Specific: "Do not delete files. Never output secrets. Validate all inputs."

❌ Vague: "Use reasoning"
✅ Specific: "Use the O.P.E.R.A. framework (Observation, Pondering, Execution, Reflexion, Adaptation)"
```

## 4. Clear Role Definition

**Rule:** Define identity with authority and specificity.

```
✅ Good: "You are the Lead Compliance Officer. Your job is to audit drafts for safety violations."
❌ Bad: "You are a helpful AI that can do many things."
```

## 5. Explicit Boundaries

**Rule:** Define what the agent CANNOT do (not just what it can).

```
You are the Optimizer.

Do NOT:
- Modify agent logic (only syntax)
- Change the persona or identity
- Delete sections

DO:
- Inject Chain of Thought
- Add concrete examples
- Improve clarity
```

---

# Optimization Process (Prompt-Optimizer)

When refining a prompt:

## Step 1: Syntax Hygiene
- Validate XML structure
- Check variable injection ({{handlebars}})
- Fix formatting and indentation

## Step 2: Cognitive Reinforcement
- Add Chain of Thought if missing
- Inject concrete examples (minimum 1)
- Clarify vague instructions

## Step 3: Critique Resolution
- If a Critic's Report exists, prioritize fixing Red (Critical) items
- Address Yellow (Warning) items
- Verify no regressions introduced

## Step 4: Output Format
```
<instruction>
[Optimized content]
</instruction>

<constraints>
[Refined boundaries]
</constraints>

<example>
[Grounded example(s)]
</example>
```

---

# Anti-Patterns (Avoid)

❌ **Instruction bloat:** 300+ line instructions  
✅ **Solution:** Reference external instruction files for shared patterns

❌ **No examples:** Complex agents without concrete I/O  
✅ **Solution:** Add minimum 1 realistic example

❌ **Vague reasoning:** No explicit process or framework  
✅ **Solution:** Reference O.P.E.R.A. or specific reasoning pattern

❌ **Hardcoded values:** Agent names, paths in instructions  
✅ **Solution:** Use `{{variables}}` for dynamic content

❌ **Missing constraints:** No clear boundaries  
✅ **Solution:** Explicit "Do Not" rules for every risk

❌ **No chain of thought:** Model jumps to conclusions  
✅ **Solution:** Include "Let's think step by step" trigger

---

# Metrics: Prompt Quality

Evaluate prompts on:

| Dimension | Poor | Good | Excellent |
|:---|:---|:---|:---|
| **Clarity** | Vague, ambiguous | Clear instructions | Precise, testable requirements |
| **Identity** | Generic helper | Clear role | Authoritative specialist |
| **Examples** | None | 1 basic example | 2-3 realistic, concrete examples |
| **Reasoning** | No framework | References pattern | Explicit O.P.E.R.A. or equivalent |
| **Constraints** | Implicit | Some explicit | Clear, comprehensive Do/Don't list |
| **Length** | 200+ lines | 30-60 lines | 20-40 lines (references external files) |
| **Coherence** | Duplicates | Some overlap | Zero duplication (uses .instructions.md) |

---

# Tools Referenced in Prompts

To reference tools in prompt instructions, use this syntax:

```
#tool:readFile       - Read file contents
#tool:editFiles      - Edit/create files
#tool:search         - Search workspace
#tool:agent          - Call another agent
#tool:runInTerminal  - Execute commands
#tool:todo           - Manage task lists
#tool:web            - Fetch web content
```

Example:
```
Use #tool:readFile to examine the codebase before proposing changes.
```

---

# Testing Your Prompt

Before deploying:

1. **Clarity Test:** Can another person understand it?
2. **Specificity Test:** Are requirements testable?
3. **Example Test:** Do examples match the instructions?
4. **Constraint Test:** Are all risks covered?
5. **Length Test:** Can content be moved to .instructions.md?

If any test fails, revise before deployment.
