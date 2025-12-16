---
name: Prompt File Design Standard
description: Specifications for creating reusable prompt files (*.prompt.md) for common development tasks
applyTo: ".github/**/*.prompt.md"
---

# Prompt File Design Standard

Prompt files (`.prompt.md`) define reusable, on-demand prompts for common development tasks like code generation, refactoring, code review, and documentation. This standard ensures consistent, maintainable, and effective prompt definitions.

---

## Overview

Prompt files (`*.prompt.md`) define single-task workflows triggered manually via `/prompt-name` in chat, providing quick access to standardized workflows without switching agents or contexts.

---

## Part 1: When to Create a Prompt

Use prompt files for **repeatable, discrete tasks**.

### Good Prompts (Examples)

**DO:**
- `/explain-code` — Explain what a code block does
- `/generate-react-form` — Scaffold a React form component
- `/security-review` — Security audit of selected code
- `/write-unit-tests` — Generate unit tests for a function
- `/refactor-to-async` — Convert callbacks to async/await
- `/generate-docs` — Create API documentation

**DON'T:**
- Generic guidance that belongs in `.github/copilot-instructions.md`
- Task requiring multi-step workflow (use agent with handoffs instead)
- Context-dependent logic without clear inputs/outputs
- Duplicate of existing prompt

### Decision Flow

```
Is it a repeatable task?
  YES → Does it need multiple agents/handoffs?
          NO → Create a prompt file ✓
          YES → Create an agent with handoffs
  NO → Add to .github/copilot-instructions.md or *.instructions.md
```

---

## Part 2: YAML Frontmatter

Prompt files use optional YAML frontmatter to configure execution context.

### Frontmatter Fields

#### `name` (Optional)

Display name in chat (accessed via `/name`); defaults to filename.

**DO:**
```yaml
name: Explain Code
```

#### `description` (Optional)

Brief summary of what the prompt does. Shown in UI.

**DO:**
```yaml
description: Explain what the selected code does in clear, accessible language
```

#### `argument-hint` (Optional)

Guidance text shown in chat input field.

**DO:**
```yaml
argument-hint: "Select code to explain, then run this prompt"
```

#### `agent` (Optional)

Specify which agent runs the prompt: `ask`, `edit`, `agent`, or custom agent name.

**DO:**
```yaml
agent: ask              # Default chat agent
agent: edit            # Code editing agent
agent: implementation  # Custom agent
```

**Default Behavior:**
- If no agent specified, uses currently selected agent
- If tools are specified and current agent is `ask`/`edit`, defaults to `agent`

#### `tools` (Optional)

Array of tools available for this prompt. Restricts or enables capabilities.

**DO:**
```yaml
tools: ['search', 'web']                # Read-only research (`web` may appear as `fetch` in some environments)
tools: ['create_file', 'replace_string_in_file']  # Code generation
tools: ['agent', 'todo']                # Reasoning only
```

**Tool Priority:**
1. Tools in prompt file (highest priority)
2. Tools from referenced agent (if specified)
3. Default tools for selected agent (lowest priority)

#### `model` (Optional)

Specific AI model to use. If omitted, uses currently selected model.

**DO:**
```yaml
model: Claude Haiku 4.5
```

### Complete Frontmatter Example

```yaml
---
name: Security Review
description: Perform security audit of selected code for vulnerabilities
argument-hint: "Select code to review for security issues"
agent: agent
tools: ['search', 'fetch', 'usages']
model: Claude Sonnet 4
---
```

---

## Part 3: Prompt File Body

The body contains the task instructions and context sent to the LLM.

### Structure

#### 1. Task Statement (Required)

Imperative description of what to accomplish.

**DO:**
```markdown
Perform a security review of the selected code.
Identify vulnerabilities, data exposure risks, and authentication issues.
Provide recommendations for each issue found.
```

**DON'T:**
```markdown
Here's a security review prompt.
You might want to look for security issues.
```

#### 2. Input Format (Required)

Describe what input the prompt expects.

**DO:**
```markdown
## Input

Selected code snippet (function, class, or module).
```

#### 3. Output Format (Required)

Specify expected deliverable structure.

**DO:**
```markdown
## Output

Markdown report with sections:
- **Issues Found:** List of security vulnerabilities (Critical/High/Medium/Low)
- **Issue Details:** For each issue:
  - Description
  - Risk level
  - Recommended fix
- **Summary:** Overall security posture
```

**DON'T:**
```markdown
## Output
Provide feedback on the code.
```

#### 4. Context References (Optional)

Link to relevant instructions or standards.

**DO:**
```markdown
## Context

Apply standards from security-review.instructions.md.
Use [OWASP Top 10](https://owasp.org/www-project-top-ten/) framework.

```

#### 5. Examples (Recommended for Complex Prompts)

Demonstrate expected behavior with real inputs/outputs.

**DO:**
```markdown
## Example

Input:
\`\`\`javascript
function fetchUserData(userId) {
  const url = `https://api.example.com/users/${userId}`;
  const response = await fetch(url);
  return response.json();
}
\`\`\`

Output:
**Issues Found:**
1. **No HTTPS enforcement:** URL should always use HTTPS
   - Risk: Medium (assumes HTTPS, not enforced)
   - Fix: Use hardcoded HTTPS URL
\`\`\`

#### 6. Additional Guidelines (Optional)

Constraints or special instructions.

**DO:**
```markdown
## Guidelines

- Focus on security and data integrity issues
- Do NOT refactor code for style; security only
- Flag even low-risk issues if easily exploitable
- Provide actionable recommendations
```

### Variable References

Prompt files support dynamic variable substitution:

#### Workspace Variables

```markdown
Workspace root: ${workspaceFolder}
Workspace name: ${workspaceFolderBasename}
```

#### File Context Variables

```markdown
Current file: ${file}
File name: ${fileBasename}
File directory: ${fileDirname}
File name without extension: ${fileBasenameNoExtension}
```

#### Selection Variables

```markdown
Selected text: ${selectedText}
Full selection: ${selection}
```

#### Input Variables

```markdown
Custom input from user: ${input:variableName}
Input with placeholder: ${input:featureName:Enter feature name}
```

### Tool References

Reference available tools using `#tool:<tool-name>` syntax:

**DO:**
```markdown
To verify code impacts, use #tool:usages to find all callers.
Use #tool:githubRepo to check community implementations.
```

---

## Part 4: Common Prompt Patterns

### Pattern 1: Code Generation

```markdown
---
name: Generate React Form
description: Generate a React form component with validation
agent: implementation
tools: ['create_file']
---

Generate a React form component that collects ${input:formName:form data}.

## Requirements

- Use TypeScript with strict mode
- Include field validation
- Handle form submission
- Include error display

## Output Format

Create file: `${fileDirname}/${fileBasenameNoExtension}.form.tsx`

Export default React functional component with:
- Props interface
- Form state management
- Validation logic
- Submit handler
```

### Pattern 2: Code Review

```markdown
---
name: Review for Performance
description: Identify performance bottlenecks and optimization opportunities
agent: ask
tools: ['search', 'fetch', 'usages']
---

Analyze the selected code for performance issues.

## Review Criteria

- Algorithmic complexity (Big-O analysis)
- Database query efficiency (N+1 queries)
- Memory usage and leaks
- Unnecessary computations
- Caching opportunities

## Output Format

Markdown report with sections:
- **Summary:** Overall performance rating
- **Issues:** List of bottlenecks with severity (Critical/High/Medium/Low)
- **Optimization Opportunities:** Ordered by impact
- **Estimated Improvement:** Time/memory savings from recommended changes
```

### Pattern 3: Documentation Generation

```markdown
---
name: Generate API Docs
description: Create comprehensive API documentation
agent: ask
tools: ['search', 'usages']
---

Generate API documentation for the selected function or class.

## Output Format

Markdown with sections:
- **Description:** What it does
- **Parameters:** Type, description, default values
- **Returns:** Type and description
- **Throws:** Exception types
- **Examples:** 2-3 usage examples
- **Related:** Links to similar functions

## Guidelines

- Use TypeScript/JSDoc syntax
- Assume intermediate developer audience
- Include edge cases in examples
- Reference related functions with #tool:usages
```

### Pattern 4: Refactoring

```markdown
---
name: Modernize to Async/Await
description: Convert callback-based code to async/await
agent: edit
tools: ['replace_string_in_file']
---

Refactor the selected code to use async/await instead of callbacks or Promises.

## Transformation Rules

- Replace `.then()` chains with `await`
- Convert error handling from `.catch()` to `try/catch`
- Make parent function `async` if needed
- Preserve functionality; no logic changes

## Output Format

Refactored code with:
- Async function signatures
- Try/catch error handling
- Await expressions
- Preserved error handling semantics

## Example

Input:
\`\`\`javascript
function fetchData(id) {
  return fetch(`/api/data/${id}`)
    .then(r => r.json())
    .catch(e => console.error(e));
}
\`\`\`

Output:
\`\`\`javascript
async function fetchData(id) {
  try {
    const response = await fetch(`/api/data/${id}`);
    return await response.json();
  } catch (error) {
    console.error(error);
  }
}
\`\`\`
```

### Pattern 5: Testing

```markdown
---
name: Write Unit Tests
description: Generate unit tests for selected function
agent: implementation
tools: ['create_file']
---

Generate comprehensive unit tests for ${input:functionName:the selected function}.

## Testing Framework

Use Jest with React Testing Library (if applicable).

## Test Coverage

- Happy path (normal input)
- Edge cases (boundary conditions)
- Error cases (exception handling)
- Integration (if applicable)

## Output Format

Create file: `${fileDirname}/${fileBasenameNoExtension}.test.ts`

Include:
- Test setup/teardown
- Mocked dependencies
- Clear test descriptions
- Assertions for each scenario

## Standards

Apply standards from testing-standards.instructions.md.
```

---

## Part 5: Best Practices

### Clarity & Specificity

Vague prompts produce vague results.

**DO:**
```markdown
Generate a React form component with:
- Name field (required, 2-100 chars)
- Email field (required, valid email)
- Submit button
- Error display for validation
- TypeScript strict mode
```

**DON'T:**
```markdown
Create a form component.
```

### Output Format Definition

Explicitly specify what success looks like.

**DO:**
```markdown
## Output Format

- Create file: `src/components/UserForm.tsx`
- Export default React component
- Include TypeScript types for props
- Include JSDoc comments for public methods
```

**DON'T:**
```markdown
Generate code that works well.
```

### Context & Reusability

Reference instructions rather than duplicating guidance.

**DO:**
```markdown
Follow standards from typescript-style.instructions.md.
Apply security guidelines from security-review.instructions.md.
```

**DON'T:**
```markdown
Use PascalCase for classes. Use 2-space indentation. Follow PEP 8...
[repeat all style rules]
```

### Variable Usage

Use variables to make prompts flexible.

**DO:**
```markdown
Generate tests for ${input:functionName:the selected function}.
Create file: ${fileDirname}/test.ts
```

**DON'T:**
```markdown
Generate tests for the function below.
Save the output to a test file.
```

---

## Part 6: Invocation Patterns

### Running Prompts

Users trigger prompts in three ways:

**Type `/` in chat:**
```
/security-review
/generate-react-form formName=LoginForm
```

**Command Palette:**
```
Chat: Run Prompt → Select security-review
```

**Play Button in Editor:**
```
Open prompt file → Click play button → Run in chat
```

### With Arguments

Prompts can accept arguments:

```markdown
/generate-react-form formName=UserProfile
/refactor-to-async functionName=fetchUserData
```

Arguments are available via `${input:variableName}` in prompt body.

---

## Part 7: File Organization

### Directory Structure

```
.github/
├── prompts/
│   ├── explain-code.prompt.md
│   ├── generate-react-form.prompt.md
│   ├── security-review.prompt.md
│   ├── write-unit-tests.prompt.md
│   ├── generate-api-docs.prompt.md
│   └── refactor-to-async.prompt.md
├── instructions/
│   ├── communication-style.instructions.md
│   ├── security-review.instructions.md
│   ├── typescript-style.instructions.md
│   └── testing-standards.instructions.md
├── agents/
│   └── implementation.agent.md
└── copilot-instructions.md
```

### Naming Convention

Prompt files follow pattern: `{name}.{domain}.prompt.md`

Examples: `explain-code.core.prompt.md`, `generate-react-form.vscode.prompt.md`, `security-review.core.prompt.md`

Use imperative verbs: `generate-`, `write-`, `review-`, `explain-`. Keep names short (≤40 chars). Use kebab-case only.

---

## Part 8: Compliance Checklist

Verify prompt file before committing:

- [ ] **Purpose Clear:** Task is discrete and repeatable
- [ ] **Frontmatter Complete:** name, description, agent, tools, model (if applicable)
- [ ] **Task Statement:** Imperative; no ambiguity about goal
- [ ] **Input Format:** Describes expected input clearly
- [ ] **Output Format:** Specifies deliverable structure and location (if file creation)
- [ ] **Context References:** Links to instructions, not duplicates
- [ ] **Variables Used:** `${variableName}` for dynamic content
- [ ] **Tool References:** `#tool:<name>` syntax for available tools
- [ ] **Communication Style:** Follows standard
  - [ ] No "please," "would you," "could you"
  - [ ] Imperative verbs ("Generate", "Write", "Review")
  - [ ] Direct constraints ("Do NOT...")
  - [ ] No hedging ("arguably", "perhaps")
  - [ ] No emojis
- [ ] **Examples Included:** Demonstrate expected input/output (for complex prompts)
- [ ] **No Duplication:** Same guidance not repeated across files
- [ ] **Instructions Referenced:** Points to `.instructions.md` for standards
- [ ] **Agent Appropriate:** Uses right agent for task (ask, edit, implementation, etc.)
- [ ] **Tools Minimal:** Only includes tools actually needed
- [ ] **Testable:** Can be manually run and verified

---

## Part 10: Examples of Well-Formed Prompts

### Example 1: Explain Code

```markdown
---
name: Explain Code
description: Explain what the selected code does in clear, accessible language
argument-hint: "Select code to explain"
agent: ask
---

Explain what the selected code does.

## Output Format

Clear explanation using:
- **Overview:** High-level summary
- **Step-by-Step:** Line-by-line breakdown
- **Key Concepts:** Explain any patterns or algorithms
- **Purpose:** What is this code trying to accomplish?
- **Related Concepts:** Similar patterns or functions to know about

## Guidelines

- Assume intermediate developer audience
- Use plain language; avoid unnecessary jargon
- Explain *why* it works this way, not just *what* it does
- Reference external concepts if needed (e.g., "This uses the Observer pattern")
```

### Example 2: Generate React Form

```markdown
---
name: Generate React Form
description: Generate a React form component with validation and error handling
argument-hint: "Enter form name (e.g., LoginForm)"
agent: implementation
tools: ['create_file']
---

Generate a React form component for ${input:formName:user input form}.

## Requirements

- TypeScript with strict mode
- Form state management with useState
- Field validation on change and submit
- Error display for each field
- Success message on submit
- Accessibility (ARIA labels, keyboard navigation)

## Output Format

Create file: `src/components/${input:formName}.tsx`

Include:
- Typed props interface
- Form state (fields and errors)
- Validation logic
- Submit handler
- Render form with fields, labels, errors
- Export as default

## Standards

Apply standards from typescript-style.instructions.md.
Use React hooks; do NOT use class components.

## Example

Input: `/generate-react-form LoginForm`

Output creates `src/components/LoginForm.tsx` with email/password fields, validation, error display.
```

### Example 3: Security Review

```markdown
---
name: Security Review
description: Perform security audit of selected code for vulnerabilities
argument-hint: "Select code to review"
agent: ask
tools: ['search', 'fetch', 'usages']
---

Perform a security review of the selected code.

## Review Criteria

- **Input Validation:** Are user inputs validated before use?
- **SQL Injection:** Are database queries parameterized?
- **Authentication & Authorization:** Are permissions checked?
- **Data Exposure:** Is sensitive data logged or exposed?
- **Dependencies:** Are packages up-to-date?
- **Secrets:** Are credentials hardcoded?
- **Error Handling:** Do error messages leak sensitive info?

## Output Format

Markdown report with sections:
- **Summary:** Pass/Fail; number of issues by severity
- **Critical Issues:** Must fix before production
- **High Issues:** Fix before next release
- **Medium Issues:** Consider fixing
- **Low Issues:** Nice-to-have improvements

Each issue includes:
- Description
- Location (file, line number)
- Risk: What could happen?
- Recommendation: How to fix

## Standards

Apply standards from security-review.instructions.md.

## Guidelines

- Flag missing validation as Critical/High
- Suggest parameterized queries for all database access
- Recommend secrets management solution for credentials
- Do NOT suggest style changes; security only
```

### Example 4: Write Unit Tests

```markdown
---
name: Write Unit Tests
description: Generate comprehensive unit tests for selected function
argument-hint: "Select function to test"
agent: implementation
tools: ['create_file']
---

Generate unit tests for ${input:functionName:the selected function}.

## Test Coverage

- Happy path (normal inputs)
- Edge cases (boundary conditions, empty inputs)
- Error cases (exceptions, rejections)
- Integration (if async or calls other functions)

## Framework

Use Jest. Include fixtures and mocked dependencies.

## Output Format

Create file: `${fileDirname}/${fileBasenameNoExtension}.test.ts`

Include:
- Imports and setup
- Test suite: `describe('${input:functionName}', ...)`
- Individual tests: `test('should ...', ...)`
- Assertions for each scenario
- Cleanup/teardown if needed

## Standards

Apply standards from testing-standards.instructions.md.

Maintain >80% code coverage.
Use descriptive test names.
Mock external dependencies (APIs, databases).

## Example

For function `calculateDiscount(price, quantity)`, generate tests for:
- Normal case (price=100, qty=5)
- Edge case (price=0, qty=0)
- Large values (price=999999, qty=9999)
```

---

## Part 11: Troubleshooting

### Prompt Not Appearing in `/` List

- Verify file is in `.github/prompts/` (workspace) or user profile
- Check file extension is `.prompt.md` (not `.prompts.md` or `.md`)
- Reload VS Code
- Check `name` field in frontmatter (used in dropdown)

### Variables Not Substituting

- Verify variable syntax: `${variableName}` (case-sensitive)
- Check variable name matches usage: `${input:formName}` in body
- Input variables require user to provide value via CLI argument

### Tools Not Available

- Verify `tools` field lists available tools correctly
- Check tool name spelling (case-sensitive)
- Some tools require specific agent (`edit` agent can't use `search`)
- Review tool availability in agent's tool list

### Prompt Produces Vague Results

- Clarify task statement; make it more specific
- Add examples of expected input/output
- Reference instructions for standards
- Specify output format more precisely
- Test prompt manually; refine based on results

---

## Tips for Success

1. **Start Simple:** Begin with discrete, well-defined tasks
2. **Use Examples:** Demonstrate expected input/output
3. **Reference Standards:** Link to instructions; don't duplicate
4. **Test Often:** Run prompts manually; refine based on results
5. **Name Clearly:** Use descriptive, action-based names
6. **Keep Focused:** One task per prompt; avoid combining unrelated tasks
7. **Document Assumptions:** Explain what the prompt expects
8. **Version Control:** Store in `.github/prompts/` for team sharing
