### **ReAct Master Planner: The Meta-Prompt Ecosystem Knowledge Base**

This document serves as the **Master Reference** for your self-improving AI ecosystem. It aggregates the methodology, file structures, agent definitions, and external research used to build your "Meta-Factory."

-----

### **1. The Core Architecture**

Your system is designed as a **Reflexive Factory**: a set of AI agents that build and improve other AI agents.

  * **The Constitution:** A global governance file that enforces rules on every interaction.
  * **The Workforce:** Three specialized agents (Architect, Critic, Optimizer) that execute the work.
  * **The Output:** High-quality, safe, and structured `.agent.md` or `.prompt.md` files.

-----

### **2. The Methodology: O.P.E.R.A.**

This is the mandatory cognitive cycle enforced by your Constitution. Every agent must follow this loop to ensure reasoning depth.

| Step | Name | Action |
| :--- | :--- | :--- |
| **O** | **Observation** | Analyze the user's raw intent and available context. |
| **P** | **Pondering** | Explore multiple approaches (Tree of Thoughts) before deciding. |
| **E** | **Execution** | Draft the initial artifact (code/prompt). |
| **R** | **Reflexion** | Critique the draft against safety and logic constraints. |
| **A** | **Adaptation** | Rewrite the draft to address the critique. |

-----

### **3. File System Reference**

The ecosystem lives in the `.github` folder of your repository.

#### **A. The Constitution (Global Rules)**

  * **Path:** `.github/copilot-instructions.md`
  * **Purpose:** The "Law" of the system.
  * **Key Rules:**
      * Start every thought with "Let's think step by step."
      * Use XML tags (`<instruction>`, `<context>`) for structure.
      * Never output code without a prior plan.

#### **B. The Agents (The Workers)**

These are the definitions for your specialized bots.

| Agent | File Path | Role | Key Capability |
| :--- | :--- | :--- | :--- |
| **@PromptArchitect** | `.github/agents/prompt-architect.agent.md` | **The Designer** | Uses "Tree of Thoughts" to select the right tool (Agent vs. Prompt). |
| **@PromptCritic** | `.github/agents/prompt-critic.agent.md` | **The Auditor** | Validates drafts against the Constitution and Safety rules. |
| **@PromptOptimizer** | `.github/agents/prompt-optimizer.agent.md` | **The Fixer** | Injects "Few-Shot Examples" and fixes syntax errors. |

#### **C. The Products (The Output)**

The factory produces these two file types:

  * **`.agent.md` (Personas):** Specialized experts with specific tools (e.g., `@DbAdmin`).
  * **`.prompt.md` (Workflows):** Reusable text templates for repetitive tasks (e.g., `/refactor`).

-----

### **4. Technical Standards & Syntax**

Your agents are trained to enforce these specific syntactic patterns.

#### **XML Delimiters**

Used to prevent "Prompt Injection" and confuse the LLM.

```markdown
<instruction>
  Act as a senior engineer.
</instruction>
<context>
  {{selection}}
</context>
```

#### **Handlebars Variables**

Used for dynamic context injection.

  * `{{selection}}`: The code currently highlighted by the user.
  * `{{file}}`: The current active file.

#### **Frontmatter**

YAML metadata required for VS Code to recognize the file.

```yaml
---
name: my-agent
description: Does cool things
model: gpt-4o
---
```

-----

### **5. External References & Research**

This ecosystem is built upon the following documentation and research concepts:

  * **VS Code Copilot Customization:**
      * *Docs:* [Customizing Copilot Overview](https://code.visualstudio.com/docs/copilot/customization/overview)
      * *Concept:* Using `.agent.md` (Chat Participants) and `.prompt.md` (Prompt Files).
  * **Prompt Engineering Techniques:**
      * *Source:* [Prompting Guide - Optimization](https://www.promptingguide.ai/guides/optimizing-prompts)
      * *Technique:* **Tree of Thoughts (ToT)** – Exploring multiple branches before answering.
      * *Technique:* **Chain of Thought (CoT)** – "Let's think step by step."
      * *Technique:* **Few-Shot Prompting** – Providing 3+ examples to guide the model.
  * **Reflexion Framework:**
      * *Concept:* An agentic loop where the model critiques its own output (Self-Correction).
  * **Model Context Protocol (MCP):**
      * *Concept:* A standard for connecting AI agents to external data (local files, APIs, database). used in the broader ecosystem for data fetching.

-----

### **6. How to Extend the Base**

If you want to add a new capability (e.g., accessing a database):

1.  **Don't write it manually.**
2.  **Ask the Factory:** `@PromptArchitect Plan an agent that can query my Postgres DB using MCP.`
3.  **Follow the Cycle:** Let the Architect design it, the Critic review it, and the Optimizer finalize it.