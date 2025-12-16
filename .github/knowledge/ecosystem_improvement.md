# Context Repository: Enhancing the Agent Ecosystem

This document consolidates insights from the existing knowledge base and provides actionable recommendations for improving the agent ecosystem. It also includes a section on integrating VS Code chat tools.

---

## 1. Key Insights from Existing Knowledge Base

### A. Reference Sources
- **Customization Overview**: Guides on customizing GitHub Copilot in VS Code, including agents, prompts, and workflows.
- **Prompt Files**: Best practices for creating reusable `.prompt.md` templates.
- **Custom Agents**: Configuration of `.agent.md` files for defining agent personas and capabilities.
- **Language Models**: Comparison of supported models (e.g., GPT-4, GPT-3.5) and their use cases.
- **Model Context Protocol (MCP)**: Connecting agents to external data sources like APIs and databases.

### B. VS Code Customization Hierarchy
- **Global Context**: `.instructions.md` files enforce workspace-wide coding standards.
- **Persona**: `.agent.md` files define agent-specific behaviors.
- **Action**: `.prompt.md` files standardize complex tasks.
- **Compute**: YAML configurations specify language models for actions.

---

## 2. Integrating VS Code Chat Tools

### A. Available Tools
1. **Custom Instructions**: Define global or file-specific rules for coding standards.
2. **Prompt Files**: Create reusable commands for repetitive tasks.
3. **Custom Agents**: Design expert personas for specific workflows.
4. **Language Model Selection**: Choose models based on task complexity and performance needs.

### B. Recommendations for Integration
- **Tool Discovery**: Provide a searchable interface for available tools.
- **Context Awareness**: Automatically suggest tools based on the active file or task.
- **Feedback Loop**: Allow users to rate and refine tool suggestions.

---

## 3. Recommendations for Enhancing the Ecosystem

### A. Documentation
- **Centralized Repository**: Maintain a single source of truth for all customization guides.
- **Examples**: Include real-world examples for each customization layer.

### B. Tooling
- **Automation**: Automate the generation of `.instructions.md` and `.prompt.md` files based on project templates.
- **Validation**: Implement linters to ensure compliance with defined standards.

### C. Collaboration
- **Community Contributions**: Encourage users to share custom agents and prompts.
- **Feedback Mechanism**: Collect user feedback to prioritize new features.

---

## 4. Chat Tools: Variables and Descriptions

The following table summarizes the available chat tools and their descriptions:

| **Chat Variable/Tool** | **Description**                                                                 |
|-------------------------|---------------------------------------------------------------------------------|
| changes             | Lists source control changes.                                                  |
| codebase            | Performs a code search in the current workspace.                               |
| createAndRunTask    | Creates and runs a new task in the workspace.                                   |
| editFiles           | Applies edits to files in the workspace.                                       |
| fetch               | Fetches content from a given web page.                                         |
| fileSearch          | Searches for files in the workspace using glob patterns.                       |
| getNotebookSummary  | Retrieves details of notebook cells.                                           |
| runSubagent         | Runs tasks in an isolated subagent context.                                    |
| todos               | Tracks implementation and progress of tasks.                                   |
| problems            | Adds workspace issues and problems from the Problems panel as context.         |
| githubRepo          | Performs a code search in a GitHub repository.                                 |
| runInTerminal       | Runs a shell command in the integrated terminal.                               |
| readFile            | Reads the content of a file in the workspace.                                  |
| searchResults       | Retrieves search results from the Search view.                                 |
| usages              | Combines "Find All References", "Find Implementation", and "Go to Definition". |

---

## 5. Tool Reference Standard

### Enforcing Tool References

To maintain consistency and clarity across the ecosystem, all prompts, instructions, and agents must refer to tools using the following format:

tool:{tool-name}

### Guidelines:
1. **Mandatory Format**: Always use the tool:{tool-name}` format when referencing tools in any `.prompt.md`, `.instructions.md`, or `.agent.md file.
2. **Clarity**: Ensure that the tool name is descriptive and matches the tool's intended functionality.
3. **Examples**:
   - Correct: tool:fetch
   - Correct: tool:runInTerminal
   - Incorrect: `fetch` (missing tool: prefix)
4. **Validation**: Implement linters or automated checks to enforce this standard during development.

### Benefits:
- **Consistency**: Ensures uniformity in tool references across the ecosystem.
- **Readability**: Makes it easier for users and developers to identify and understand tool usage.
- **Integration**: Facilitates seamless integration with VS Code's chat tools and other automation workflows.

---

**How to Use This Document**:
- Reference the insights section when designing or optimizing agents.
- Follow the integration guidelines to make the most of VS Code chat tools.
- Implement the recommendations to continuously improve the ecosystem.