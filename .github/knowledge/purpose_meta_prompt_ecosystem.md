### **ReAct Master Planner: The Meta-Prompt Ecosystem Knowledge Base**

This document serves as the **Master Reference** for your self-improving AI ecosystem. It aggregates the methodology, file structures, agent definitions, and external research used to build your "Meta-Factory."

-----

### **1. The Core Architecture**

Your system is designed as a **Reflexive Factory**: a set of AI agents that build and improve other AI agents.

  * **The Constitution:** A global governance file that enforces rules on every interaction.
  * **The Workforce:** A network of specialized agents (Architect, Critic, Optimizer, Orchestrator, etc.) that execute the work.
  * **The Output:** High-quality, safe, and structured `.agent.md`, `.prompt.md`, or `.instructions.md` files.

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

#### **A. The Governance Layers**

**Layer 1: The Constitution (Global Rules)**
  * **Path:** `.github/copilot-instructions.md`
  * **Purpose:** The "Law" of the system.

**Layer 2: The Instructions (Contextual Rules)**
  * **Path:** `.github/instructions/*.instructions.md`
  * **Purpose:** Modular, reusable rule sets for specific domains.
  * **Key Files:**
      * `instruction-governance.instructions.md`: Rules for writing rules.
      * `safety-standards.instructions.md`: Universal safety constraints.
      * `opera.instructions.md`: The core reasoning loop.
      * `agent-design.instructions.md`: Guidelines for designing agents.
      * `prompt-engineering.instructions.md`: Optimization techniques.

#### **B. The Agents (The Workers)**

These are the definitions for your specialized bots.

| Agent | File Path | Role | Key Capability |
| :--- | :--- | :--- | :--- |
| **@EcosystemOrchestrator** | `.github/agents/ecosystem-orchestrator.core.agent.md` | **The Manager** | Coordinates complex multi-agent workflows. |
| **@MasterPlanner** | `.github/agents/master-planner.core.agent.md` | **The Strategist** | Breaks down high-level goals into actionable plans. |
| **@MetaPromptArchitect** | `.github/agents/meta-prompt-architect.creation.agent.md` | **The Designer** | Designs new agents and prompts. |
| **@MetaPromptCritic** | `.github/agents/meta-prompt-critic.quality.agent.md` | **The Auditor** | Validates drafts against governance and safety. |
| **@MetaPromptOptimizer** | `.github/agents/meta-prompt-optimizer.creation.agent.md` | **The Fixer** | Optimizes prompts and injects few-shot examples. |
| **@Reasoner** | `.github/agents/reasoner.core.agent.md` | **The Thinker** | Performs deep logical analysis and deduction. |
| **@SyntheticAnalyst** | `.github/agents/synthetic-analyst.core.agent.md` | **The Researcher** | Analyzes data and patterns. |
| **@MetaPrompter** | `.github/agents/meta-prompter.workflow.agent.md` | **The Executor** | Executes prompt generation workflows. |
| **@HandoffOptimizer** | `.github/agents/handoff-optimizer.workflow.agent.md` | **The Router** | Optimizes agent-to-agent handoffs. |
| **@PatternValidator** | `.github/agents/pattern-validator.quality.agent.md` | **The Checker** | Validates structural patterns. |
| **@EcosystemAuditor** | `.github/agents/ecosystem-auditor.infrastructure.agent.md` | **The Inspector** | Audits the entire ecosystem for consistency. |
| **@ConflictDetection** | `.github/agents/conflict-detection.infrastructure.agent.md` | **The Judge** | Detects conflicts in instructions. |
| **@MetaLifecycleManager** | `.github/agents/meta-lifecycle-manager.infrastructure.agent.md` | **The Maintainer** | Manages the lifecycle of agents. |
| **@MetaSpecialistFactory** | `.github/agents/meta-specialist-factory.creation.agent.md` | **The Builder** | Creates specialized agents. |

#### **C. The Products (The Output)**

The factory produces these file types:

  * **`.agent.md` (Personas):** Specialized experts with specific tools.
  * **`.prompt.md` (Workflows):** Reusable text templates for repetitive tasks.
  * **`.instructions.md` (Rules):** Modular governance and logic modules.

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
2.  **Ask the Factory:** `@MetaPromptArchitect Plan an agent that can query my Postgres DB using MCP.`
3.  **Follow the Cycle:** Let the Architect design it, the Critic review it, and the Optimizer finalize it.