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

## Available Agents

| Agent | Role | Description |
|-------|------|-------------|
| `@master-planner` | **The Architect** | The central hub for planning and orchestration. Uses the O.P.E.R.A. framework. |
| `@reasoner` | **The Logic Engine** | Verifies logic using deductive reasoning and formal proofs. |
| `@synthetic-analyst` | **The Researcher** | Performs deep systemic analysis and research. |
| `@prompt-architect` | **The Builder** | Designs and scaffolds new agents and prompts. |
| `@prompt-critic` | **The Auditor** | Reviews drafts for safety, logic, and compliance. |
| `@prompt-optimizer` | **The Polisher** | Refines prompts for maximum performance. |
| `@meta-prompter` | **The Structural Engine** | Solves problems using abstract syntax and meta-prompting techniques. |
| `@handoff-optimizer` | **The Workflow Specialist** | Optimizes the connections between agents. |

Add a new agent
 - Create a Markdown file with the `.agent.md` extension and place it in `.github/agents/`.
 - Include YAML frontmatter to define `name`, `description`, `tools`, `handoffs`, and other settings. See VS Code docs: https://code.visualstudio.com/docs/copilot/customization/custom-agents

Contributing
 - See CONTRIBUTING.md for contribution guidelines and agent templates.

License
 - This project is licensed under the terms in the LICENSE file.
