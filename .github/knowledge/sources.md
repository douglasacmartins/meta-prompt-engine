# Meta-Prompt Ecosystem: Reference Sources

This document provides annotated links to the official documentation and key resources that guide the architecture, design, and operation of this ecosystem. Use it as a quick reference for best practices, syntax, and technical standards.

---

## Copilot Customization

- [Copilot Customization Overview](https://code.visualstudio.com/docs/copilot/customization/overview)
	- **Purpose:** High-level introduction to customizing GitHub Copilot in VS Code, including agents, prompts, and workflows.
	- **Use When:** You need to understand the overall customization capabilities and architecture.

- [Custom Instructions](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
	- **Purpose:** Explains how to define global and per-agent instructions using XML delimiters and Handlebars variables.
	- **Use When:** Writing or editing `.instructions.md` files or setting up agent constraints.

- [Prompt Files](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
	- **Purpose:** Details the structure, syntax, and best practices for `.prompt.md` files (workflows/templates).
	- **Use When:** Creating or refining reusable prompt templates for repetitive tasks.

- [Custom Agent](https://code.visualstudio.com/docs/copilot/customization/custom-agents)
	- **Purpose:** Describes how to define and configure custom agents (`.agent.md`), including frontmatter and capabilities.
	- **Use When:** Designing new personas or expert agents for the ecosystem.

## Language Models

- [Language Models](https://code.visualstudio.com/docs/copilot/customization/language-models)
	- **Purpose:** Lists available language models, their capabilities, and how to specify them in agent/prompt files.
	- **Use When:** Selecting or comparing models for specific tasks or performance needs.

- [Model Comparison](https://docs.github.com/en/copilot/reference/ai-models/model-comparison)
	- **Purpose:** Provides a side-by-side comparison of Copilot-supported models (e.g., GPT-4, GPT-3.5, GPT-4o).
	- **Use When:** Deciding which model to use for a new agent or workflow.

## Model Context Protocol (MCP)

- [MCP Servers](https://code.visualstudio.com/docs/copilot/customization/mcp-servers)
	- **Purpose:** Explains how to connect agents to external data sources (files, APIs, databases) using MCP.
	- **Use When:** Building agents that require access to data beyond the current workspace.

---

**How to Use This File:**
- Reference the relevant section when designing, critiquing, or optimizing agents and prompts.
- Follow the official documentation for syntax, structure, and best practices.
- Update this file as new official resources become available.