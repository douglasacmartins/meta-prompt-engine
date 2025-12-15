# meta-prompt-engine
This is a repository to create and validade autonomous agentic worlflow for high level prompt creation
# meta-prompt-engine
This repository stores and documents reusable VS Code custom agents (`.agent.md`) for Copilot-style workflows.

Purpose
- Host workspace-level agent definitions in `.github/agents/` so team members can pick specialized agents in VS Code Chat.
- Provide guidance for adding, reviewing, and sharing custom agents across workspaces or organizations.

Quick start
 - Open this repository in VS Code.
 - The repository's agents are located at [.github/agents](.github/agents).
 - To use an agent: open the Chat view in VS Code, select Configure Custom Agents → Create or choose an agent. VS Code auto-detects `.agent.md` files in `.github/agents`.

## Meta-Prompt Agent Generation Framework (MPAGF)

The system operates as a comprehensive framework for generating flawless AI agents through structured layers:

### **Framework Architecture**
- **Layer 0:** Meta-Governance (rules for writing rules)
- **Layer 1:** Foundation Instructions (core principles)  
- **Layer 2:** Specialized Instructions (domain expertise)
- **Layer 3:** Generation Templates (reusable prompts)
- **Layer 4:** Agent Ecosystem (specialized implementations)

### **Available Agents**

| Agent | Framework Role | Description |
|-------|------|-------------|
| `@master-planner` | **System Orchestrator** | Strategic planning and multi-agent composition using O.P.E.R.A. framework |
| `@meta-specialist-factory` | **Agent Generator** | Creates specialized agents for novel domains via MPAGF pipeline |
| `@meta-prompt-critic` | **Quality Gate** | 5-layer validation pipeline ensuring flawless agent generation |
| `@meta-prompt-architect` | **Component Designer** | Designs framework components (agents, prompts, instructions) |
| `@meta-prompt-optimizer` | **Performance Enhancer** | Optimizes prompts and agents for maximum effectiveness |
| `@meta-prompter` | **Structure Engine** | Defines abstract syntax and structural requirements |
| `@ecosystem-orchestrator` | **Router** | Intelligent delegation and workflow management |
| `@synthetic-analyst` | **Deep Analysis** | Systemic analysis and research using hermeneutic cycles |
| `@reasoner` | **Logic Validation** | Deductive reasoning and formal verification |

Add a new agent
 - Create a Markdown file with the `.agent.md` extension and place it in `.github/agents/`.
 - Include YAML frontmatter to define `name`, `description`, `tools`, `handoffs`, and other settings. See VS Code docs: https://code.visualstudio.com/docs/copilot/customization/custom-agents

Contributing
 - See CONTRIBUTING.md for contribution guidelines and agent templates.

License
 - This project is licensed under the terms in the LICENSE file.
