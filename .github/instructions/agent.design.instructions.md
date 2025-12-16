---
name: Agent File Design Standard
description: Specifications for creating well-structured custom agent definition files (*.agent.md)
applyTo: ".github/**/*.agent.md"
---

# Agent File Design Standard

Agent definition files (`.agent.md`) configure custom agents in VS Code by specifying identity, capabilities, behavior, and handoff workflows. This standard ensures consistent, scalable agent architecture.

---

## File Structure Overview

Agent files follow this structure:

1. **YAML Frontmatter** — Metadata (name, description, tools, handoffs)
2. **Markdown Body** — Instructions, process, examples, constraints
3. **Handoff Definitions** — Sequential workflow routing

---

## Part 1: YAML Frontmatter

The frontmatter must include these fields:

### Required Fields

#### `description`
Brief, imperative summary of agent purpose. Shown as placeholder text in chat.

**DO:**
```yaml
description: RADD Logic Validator—Formal logical verification using Research→Abductive→Dialectic→Deductive
```

**DON'T:**
```yaml
description: A validator that checks logic using various reasoning methods
```

#### `name`
Agent display name (optional; defaults to filename).

**DO:**
```yaml
name: logic-validator
```

#### `tools`
Array of available tool names. Explicitly restrict tools to agent's scope.

**DO:**
```yaml
tools: ['agent', 'todo']  # Pure reasoning; no retrieval
```

**DON'T:**
```yaml
tools: '*'  # Avoid; be explicit about capabilities
```

### Optional Fields

#### `argument-hint`
Guidance text shown in chat input (optional).

**DO:**
```yaml
argument-hint: "Paste logic or plan to validate"
```

#### `model`
Specific AI model (optional; uses currently selected model if omitted).

**DO:**
Always omit this field to use default model.

#### `target`
Environment: `vscode` (default) or `github-copilot`.

**DO:**
```yaml
target: vscode
```

#### `infer`
Enable agent as subagent (default `true`).

```yaml
infer: true
```

#### `mcp-servers`
MCP server configurations for GitHub Copilot deployments (optional).

```yaml
mcp-servers:
  - name: filesystem
    config: {...}
```

### Handoffs Field

Define sequential workflow transitions. See **Part 3: Handoff Definitions**.

---

## Part 2: Markdown Body

Agent instructions comprise the file body. Structure follows this hierarchy:

### Section 1: Identity & Purpose

Establish agent role and scope immediately.

**DO:**
```markdown
You are the RADD Logic Validator.
Validate logical soundness using formal RADD methodology.
Flag contradictions, axiom violations, and logical gaps.
```

**DON'T:**
```markdown
This is a reasoning engine that might help you think through problems.
It's designed to validate logic in various ways.
```

### Section 2: Process / Methodology

Define explicit stages or phases. Use imperative verbs.

**DO:**
```markdown
## Process

Stage 1: Research—Extract facts, establish premises
Stage 2: Abductive—Infer hidden mechanisms and causal relationships
Stage 3: Dialectic—Identify contradictions; test axioms against evidence
Stage 4: Deductive—Verify logical soundness; generate conclusions
```

**DON'T:**
```markdown
## Process
The agent follows a four-step process. First, it researches things...
```

### Section 3: Constraints

Direct mandates about tool usage, scope, and behavior.

**DO:**
```markdown
## Constraints

Do NOT use retrieval tools; pure reasoning only.
Do NOT generate code; validate logic exclusively.
Do NOT make assumptions; flag missing premises.
Delegate data retrieval via handoffs to [agent-name].
```

**DON'T:**
```markdown
## Constraints
You could try not using search tools. It might be helpful to focus on reasoning.
```

### Section 4: Output Format

Specify expected deliverables and structure.

**DO:**
```markdown
## Output Format

Report: Markdown document with sections:
- **Premises:** Facts assumed or extracted
- **Logical Chain:** Step-by-step reasoning
- **Contradictions:** Any axiom violations (if found)
- **Validity Assessment:** Pass/Fail with rationale
```

**DON'T:**
```markdown
## Output
Feel free to provide your findings in whatever format seems best.
```

### Section 5: Examples (Optional)

Demonstrate agent behavior with concrete scenarios.

**DO:**
```markdown
## Example

User Input: Validate this plan for logical consistency.

Output:
**Premises Identified:**
1. System must handle 1000 req/sec (stated)
2. Current throughput: 100 req/sec (measured)
...
```

### Section 6: Handoff Triggers (Optional)

Explain when to hand off to other agents.

**DO:**
```markdown
## When to Hand Off

Hand off to @data-collector when:
- Premises require external data
- Axioms need empirical verification

Hand off to @code-reviewer when:
- Logic validation passes and implementation review needed
```

---

## Part 3: Handoff Definitions

Handoffs enable guided workflow transitions. Define in frontmatter `handoffs` array.

### Handoff Structure

```yaml
handoffs:
  - label: [button-text]         # Display label (required)
    agent: [target-agent-id]     # Target agent name (required)
    prompt: [prefilled-text]     # User message to send (required)
    send: [true|false]           # Auto-submit flag (optional; default false)
```

### Design Principles

**One-Way Flow:** Handoffs define → not ← relationships. Avoid circular dependencies.

**Explicit Prompts:** Pre-fill prompts with sufficient context for next agent.

**Non-Disruptive:** Default `send: false`; let user review before switching.

### Examples

#### Example 1: Planning → Implementation

```yaml
handoffs:
  - label: Implement Plan
    agent: implementation
    prompt: Implement the plan outlined above.
    send: false
```

**Rationale:** User reviews plan in planner agent, then explicitly switches to implementation with context preserved.

#### Example 2: Implementation → Review

```yaml
handoffs:
  - label: Review Implementation
    agent: code-review
    prompt: Review the implementation above for security, performance, and code quality.
    send: false
```

#### Example 3: Auto-Routing

```yaml
handoffs:
  - label: Validate Logic
    agent: logic-validator
    prompt: Validate the plan above for logical consistency and axiom violations.
    send: false
```

**Rationale:** Auto-routing is unsafe. Require manual handoff initiation.

### Handoff Best Practices

**DO:**
- Reference prior context explicitly in prompt
- Use descriptive labels ("Implement Plan", not "Next")
- Provide at least 1-2 handoffs for multi-step workflows
- Default to `send: false` for high-stakes transitions (implementation, deployment)

**DON'T:**
- Create circular handoff chains (A → B → C → A)
- Use vague prompts ("Now do your thing")
- Handoff to agents that duplicate current agent's scope
- Use auto-routing without clear deterministic criteria

---

## Part 4: Tool Usage

Restrict tools to agent's scope. Tools define capabilities and constraints.

### Tool Categories

**Read-Only Tools:**
```yaml
tools: ['search', 'web', 'usages', 'githubRepo']
```
Use for: Planning, analysis, review agents.

**Reasoning Tools:**
```yaml
tools: ['agent', 'todo']
```
Use for: Logic validation, reasoning, synthesis.

**Modification Tools:**
```yaml
tools: ['create_file', 'replace_string_in_file', 'run_in_terminal']
```
Use for: Implementation, code generation agents.

**Mixed Toolset (Specialized):**
```yaml
tools: ['search', 'web', 'agent', 'create_file']
```
Use for: Research + implementation agents.

### Tool Priority

1. Tools in **prompt file** (if used)
2. Tools in **referenced agent** (if referenced)
3. Default tools for agent

### MCP Tool Inclusion

Include all tools from an MCP server:

```yaml
tools: ['filesystem/*', 'git/*']
```

---

## Part 5: Instruction Examples

### Example 1: Planning Agent

```markdown
---
description: Generate implementation plan for new features or refactoring.
name: planner
tools: ['search', 'fetch', 'githubRepo']
handoffs:
  - label: Start Implementation
    agent: implementation
    prompt: Implement the plan outlined above.
    send: false
---

You are the Planning Agent.
Generate detailed implementation plans without making code edits.

## Process

1. Analyze Requirements—Extract functional and non-functional requirements
2. Decompose Features—Break into atomic tasks with dependencies
3. Design Architecture—Propose modular structure
4. Estimate Effort—Provide effort/complexity estimates
5. Identify Risks—Flag technical, schedule, and dependency risks

## Constraints

Do NOT generate code or make edits.
Pure planning; research-only tools.
Delegate implementation to @implementation agent.

## Output Format

Markdown document with sections:
- Overview
- Requirements (functional and non-functional)
- Implementation Steps (ordered, atomic)
- Architecture Diagram (ASCII or Markdown table)
- Risk Assessment
- Effort Estimates
```

### Example 2: Logic Validation Agent

```markdown
---
description: RADD Logic Validator—Formal logical verification
name: logic-validator
tools: ['agent', 'todo']
handoffs:
  - label: Analyze System Impact
    agent: synthetic-analyst
    prompt: Analyze system-level impact of this logic and identify dependencies.
    send: false
---

You are the RADD Logic Validator.
Validate logical soundness using Research→Abductive→Dialectic→Deductive methodology.

## Methodology

Stage 1: Research—Extract facts; establish premises
Stage 2: Abductive—Infer mechanisms; identify assumptions
Stage 3: Dialectic—Challenge axioms; identify contradictions
Stage 4: Deductive—Verify logical chains; generate conclusions

## Constraints

Pure reasoning only; do NOT retrieve data.
Do NOT execute code.
Delegate data retrieval to @data-collector.
Flag missing premises explicitly.

## Output

Markdown report:
- **Premises Identified:** Facts and assumptions
- **Logical Chain:** Step-by-step reasoning
- **Contradictions Found:** Any axiom violations (if present)
- **Validity:** PASS / FAIL with rationale
```

### Example 3: Code Review Agent

```markdown
---
description: Security and quality code review
name: code-reviewer
tools: ['fetch', 'usages', 'githubRepo']
handoffs:
  - label: Fix Issues
    agent: implementation
    prompt: Fix the issues identified above.
    send: false
---

You are the Code Review Agent.
Identify security vulnerabilities, performance issues, and code quality problems.

## Review Criteria

Security: SQL injection, XSS, authentication, authorization, data exposure
Performance: Inefficient algorithms, N+1 queries, unnecessary computations
Maintainability: Code clarity, documentation, test coverage, SOLID principles
Standards: Language idioms, naming conventions, code style

## Constraints

Do NOT make code edits; review only.
Read-only tool access.
Flag issues with severity (Critical, High, Medium, Low).
Delegate fixes to @implementation agent.

## Output Format

Markdown report with sections:
- **Summary:** Issue count by severity
- **Critical Issues:** Must fix before merge
- **Recommendations:** Nice-to-have improvements
- **Positive Notes:** Well-implemented patterns
```

---

## Part 6: Compliance Checklist

Verify agent file before committing:

- [ ] **Frontmatter Complete:** name, description, tools, handoffs (if applicable)
- [ ] **Tools Explicit:** Tools list not empty or overly broad
- [ ] **Identity Clear:** Agent purpose stated in opening line
- [ ] **Process Defined:** Stages/steps explained if applicable
- [ ] **Constraints Listed:** Do NOT statements for out-of-scope activities
- [ ] **Output Format Specified:** Expected structure for deliverables
- [ ] **Handoffs Logical:** Sequential workflow without cycles
- [ ] **Prompts Prefilled:** Handoff prompts include context from current agent
- [ ] **Communication Style:** Follows imperative, direct language standard
  - [ ] No "please," "could you," "would you"
  - [ ] Imperative verbs in process steps
  - [ ] Direct constraint language ("Do NOT...")
  - [ ] No hedging ("arguably," "perhaps," "might")
- [ ] **Examples Included:** Demonstrate expected behavior (for complex agents)
- [ ] **Tool Consistency:** Tools match agent's actual scope (no retrieval in pure reasoning agents)

---

## File Location & Naming

**Workspace-Shared Agents:**
```
.github/agents/*.agent.md
```
**Domain-Specific Agents:**
- Use kebab-case domain naming: `{agent-name}.{agent-domain}.agent.md`
- Domain reflects specialization (e.g., `reasoning`, `core`, `infrastructure`)

**Agent Naming Convention:**
- Use kebab-case: `logic-validator.agent.md`, `code-reviewer.agent.md`
- Name reflects agent role, not generic "agent"
- Avoid redundant suffixes: `logic.agent.md` (not `logic-agent.agent.md`)
- If specialized, include domain: `{agent-name}.{domain}.agent.md`

---

## Appendix: Tool Reference

### Built-In Tools (VS Code)

| Tool | Purpose | Read-Only? |
|------|---------|-----------|
| `search` | Full-text workspace search | ✓ |
| `web` | Fetch web page content | ✓ |
| `fetch` | Fetch web page content (alias in some environments) | ✓ |
| `usages` | Find symbol usages | ✓ |
| `githubRepo` | Search GitHub repositories | ✓ |
| `agent` | Delegate to other agents | ✓ |
| `todo` | Manage task lists | ✓ |
| `create_file` | Create new files | ✗ |
| `replace_string_in_file` | Edit file content | ✗ |
| `run_in_terminal` | Execute terminal commands | ✗ |

### MCP Tools (Model Context Protocol)

Refer to specific MCP server documentation for available tools.

---

## Examples of Well-Formed Agents

Reference existing `.agent.md` files in `.github/agents/` for patterns:

- `opera.reasoning.agent.md` — Multi-hop decomposition
- `reasoner.reasoning.agent.md` — RADD formal logic validation
- `synthetic-analyst.reasoning.agent.md` — Hermeneutic systemic analysis
- `master-planner.core.agent.md` — OPERA orchestration
- `ecosystem-orchestrator.core.agent.md` — Router/delegation

All follow this standard and demonstrate best practices.

---

## Plan Agent Integration

#### YAML Frontmatter
- **Required Fields**:
  - `name`: Agent display name (e.g., `Plan`).
  - `description`: Brief summary of the agent's purpose.
  - `tools`: Explicitly list tools required for planning tasks.
  - `handoffs`: Define workflow transitions with clear prompts.

#### Markdown Body
- **Identity**:
  - Clearly define the agent's role and scope.
  - Example: "You are a PLANNING AGENT, responsible for creating actionable plans."

- **Workflow**:
  - Include `<workflow>` tags to outline the planning process.
  - Example:
    ```markdown
    <workflow>
    1. Context gathering and research.
    2. Drafting a concise, actionable plan.
    3. Iterating based on user feedback.
    </workflow>
    ```

- **Stopping Rules**:
  - Use `<stopping_rules>` to prevent the agent from executing implementation tasks.
  - Example:
    ```markdown
    <stopping_rules>
    STOP IMMEDIATELY if you consider starting implementation.
    </stopping_rules>
    ```

- **Examples**:
  - Provide concrete scenarios demonstrating the agent's planning capabilities.
