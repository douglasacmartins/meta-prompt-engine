# Agent Knowledge Base: VS Code Copilot Customization

## 1. Hierarchy of Customization
To generate the correct response, you must understand how VS Code layers context.

| Layer | Feature | File Extension | Scope | Trigger |
| :--- | :--- | :--- | :--- | :--- |
| **1. Global Context** | **Custom Instructions** | .instructions.md | Workspace/User | Automatic (Invisible) |
| **2. Persona** | **Custom Agents** | .agent.md | Workspace/User | @AgentName |
| **3. Action** | **Prompt Files** | .prompt.md | Workspace/User | /Command |
| **4. Compute** | **Language Models** | (Config in YAML) | Per Action | Config or UI Picker |

---

## 2. Custom Instructions (Global Context)
**Purpose:** Enforce coding standards, style guides, and behavior across *all* interactions.

### A. Workspace-Wide Instructions
**File Path:** .github/copilot-instructions.md
**Behavior:** Automatically added to the system prompt of every chat request in the workspace.

**Example Content:**
```markdown
# Project Standards
* **Language:** Always use TypeScript 5.0+.
* **Style:** Prefer functional programming patterns; avoid classes unless necessary.
* **Testing:** All new functions must include Vitest unit tests.
* **Tone:** Be concise. Do not explain standard language features.
```

### B. Scoped Instructions (File-Specific)
**File Path:** .github/instructions/*.instructions.md
**Purpose:** Apply rules only when specific files are active or referenced.

**Schema:**
```yaml
---
description: "SQL Standards"
applyTo: "**/*.sql" # Glob pattern for file matching
---
# SQL Rules
* Use Common Table Expressions (CTEs) instead of subqueries.
* Keywords must be UPPERCASE.
```

---

## 3. Prompt Files (Reusable Tasks)
**Purpose:** Define reusable, parameterized commands (like functions) to standardize complex tasks.
**Location:** .github/prompts/ (Workspace) or Profile Folder (User).

### Schema (YAML + Markdown)
```yaml
---
name: generate-docs              # The slash command (e.g., /generate-docs)
description: "Generates JSDoc"   # UI description
agent: agent                     # 'agent' (default), 'edit', or specific custom agent
model: gpt-4o                    # Force a specific model (optional)
tools: ['githubRepo', 'search']  # Enable specific tools
argument-hint: "[func_name]"     # Hint for user input
---
Generate JSDoc documentation for the function `${input:funcName}`.
Include:
1. @param types
2. @returns description
3. An example usage block.

Context: ${selection}
```

### Key Variables
| Variable | Description |
| :--- | :--- |
| ${selection} | The currently selected text in the active editor. |
| ${file} | The full content of the currently active file. |
| ${input:varName} | Prompts the user to type a value for varName. |
| ${workspaceFolder} | The root path of the current project. |

### Example: Unit Test Generator
**File:** .github/prompts/test-gen.prompt.md
```markdown
---
name: test
description: Generate Vitest tests
tools: ['githubRepo']
model: gpt-4o
---
# Context
Analyze the following file: ${file}

# Goal
Write a Vitest test suite for the selected code: ${selection}.

# Constraints
* Use describe blocks for grouping.
* Mock external dependencies using vi.mock.
* Cover edge cases (null inputs, empty arrays).
```

---

## 4. Custom Agents (Specialized Personas)
**Purpose:** Create a dedicated "Expert" with a persistent role, restricted tools, and specific workflow logic.
**Location:** .github/agents/

### Schema (YAML + Markdown)
```yaml
---
name: Security                 # Trigger via @Security
description: "AppSec Expert"
tools: ['search', 'fetch']     # Can RESTRICT tools (e.g., no write access)
model: o1-preview              # Use reasoning models for complex analysis
handoffs:                      # Chained workflows
  - label: "Fix Issues"
    agent: @workspace
    prompt: "Apply the fixes recommended above."
    send: false
---
# Identity
You are a Security Engineer. You are paranoid and thorough.
Your goal is to find vulnerabilities in the code provided.

# Protocol
1. Analyze the logic for Injection (SQLi, XSS).
2. Check for broken Access Control.
3. Output findings in a Markdown table.
```

### Example: Planning Agent (The "Architect")
**File:** .github/agents/planner.agent.md
```markdown
---
name: Planner
description: "Architecture & Roadmap"
model: o1-preview
tools: ['search', 'githubRepo']
handoffs:
  - label: "Implement"
    agent: @workspace
    prompt: "Implement the plan defined in the previous turn."
---
# Role
You are a Software Architect. You DO NOT write code. You only write PLANS.

# Output Format
Provide a USAGE.md structure:
1. High Level Design
2. API Interface Definitions
3. Step-by-Step Implementation Guide
```

---

## 5. Language Models (Compute Selection)
**Purpose:** Select the optimal "Brain" for the specific task.

### Configuration Methods
1.  **In UI:** User selects manually via the "Model Picker" in Chat.
2.  **In Files (YAML):** Agents/Prompts can force a model.

### Recommended Model Strategy
| Task Type | Recommended Model | Why? |
| :--- | :--- | :--- |
| **Simple Code Gen** | gpt-4o / claude-3.5-sonnet | Fast, high throughput. |
| **Complex Planning** | o1-preview / o1-mini | High reasoning capability, slower. |
| **Refactoring** | claude-3.5-sonnet | Large context window, good at preserving structure. |

### Example: Enforcing Reasoning
In .prompt.md or .agent.md:
```yaml
---
name: deep-refactor
model: o1-preview
---
(Instructions for a complex refactor...)
```

---

## 6. Interaction & Precedence Rules
When a user executes a command, VS Code resolves conflicts using this priority:

### A. Tool Availability
1.  **Prompt File Tools** (tools: [...] in .prompt.md) - *Highest Priority*
2.  **Custom Agent Tools** (tools: [...] in .agent.md)
3.  **Default Agent Tools** (Standard VS Code tools) - *Lowest Priority*

### B. Context Layering (The Prompt Stack)
The LLM receives instructions in this order (top to bottom):
1.  copilot-instructions.md (Global constraints)
2.  agent.md (Persona definition)
3.  prompt.md (Task instructions)
4.  User Input (Chat message)

**Implication:** A Custom Instruction (Layer 1) saying "Never use any" will override a Prompt File (Layer 3) asking to "Use any for speed", unless the Agent (Layer 2) explicitly overrides strict mode.