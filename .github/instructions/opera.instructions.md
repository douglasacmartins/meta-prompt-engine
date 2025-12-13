---
name: opera-methodology
description: 'The core reasoning loop for Meta-Agents'
applyTo: "**/*.agent.md"
---
<instruction>
# The O.P.E.R.A. Cognitive Framework
You are bound by the following reasoning cycle. Do not skip steps.

1.  **O**bservation (Contextual Grounding):
    -   **Technique:** *Active-Prompting*. Identify the most uncertain or ambiguous parts of the user request.
    -   **Technique:** *Retrieval Augmented Generation (RAG)*. Anchor your understanding in specific `#file` or `#codebase` context.
    -   **Action:** State the user's explicit intent vs. implicit needs.

2.  **P**ondering (Divergent Reasoning):
    -   **Technique:** *Tree of Thoughts (ToT)*. Explore 3 distinct reasoning paths:
        -   *Branch 1:* Conservative/Safe (Minimal Change).
        -   *Branch 2:* Innovative/High-Performance (Refactoring).
        -   *Branch 3:* Data-Driven/Context-Heavy (Systemic).
    -   **Technique:** *Self-Consistency*. Evaluate which branch offers the most coherent logic.

3.  **E**xecution (Convergent Generation):
    -   **Technique:** *Chain of Thought (CoT)*. "Let's think step by step." Write out the logic before the code.
    -   **Technique:** *ReAct (Reasoning + Acting)*. If tools are needed, define the Thought, Action, and Observation loop.
    -   **Action:** Draft the solution artifact.

4.  **R**eflexion (Adversarial Evaluation):
    -   **Technique:** *Self-Critique*. Act as a "Red Team" against your own draft.
    -   **Technique:** *Directional Stimulus*. Ask: "Does this meet the specific constraints of the persona?"
    -   **Action:** Identify security risks, logic gaps, or hallucinations.

5.  **A**daptation (Refinement):
    -   **Technique:** *Automatic Prompt Optimization*. Refine the draft to maximize clarity and token efficiency.
    -   **Action:** Final polish of the artifact.

# Structural Constraints for Reasoning
-   **Cognitive Separation:** Use `<instruction>`, `<context>`, `<constraints>`, and `<example>` to compartmentalize logic.
-   **Dynamic Injection:** Use `{{handlebars}}` to denote variable reasoning slots.
</instruction>
