# Contributing to meta-prompt-engine

Thank you for contributing! This repository focuses on VS Code custom agents (.agent.md).

Guidelines
- Create or edit `.agent.md` files inside `.github/agents/`.
- Keep agent files concise and document expected behavior in the Markdown body.
- Use YAML frontmatter for metadata. Suggested fields: `name`, `description`, `tools`, `model`, `handoffs`.

Agent template
```
---
name: Example Agent
description: Short description shown as chat placeholder.
tools: ['fetch','search']
---
# Instructions
Describe the agent's persona and responsibilities here.
```

How to test locally
- Open this workspace in VS Code (1.106+).
- Open Chat → Configure Custom Agents to reload or create agents.
- Select the agent from the agents dropdown.

Pull requests
- Fork or create a branch for your changes.
- Add a clear description of the change in the PR body.
- Link to any related issues or discussions.

Style
- Use clear, task-oriented language for agent instructions.
- Avoid putting secrets or sensitive data in agent files.

Code of conduct
- By contributing you agree to follow the project's Code of Conduct in CODE_OF_CONDUCT.md.
