---
name: Instructions File Design Standard
description: Specifications for creating custom instructions files (*.instructions.md) that guide AI behavior
applyTo: ".github/**/*.instructions.md"
---

# Instructions File Design Standard

Instructions files (`.instructions.md`) configure AI behavior for specific tasks, file types, or projects by providing guidelines, rules, and context. This standard ensures consistent, reusable, and maintainable instruction definitions.

---

## Overview

Instructions files (`*.instructions.md`) apply conditionally based on glob patterns, enabling targeted guidance for specific file types, folders, or domains.

---

## Part 1: File Purpose & Scope

Instructions files address specific problem domains:

### Examples of Good Instruction Scopes

**DO:**
- Language-specific coding standards (`*.py` Python files)
- Framework conventions (`*.vue` Vue.js templates)
- Project standards (security review guidelines)
- Domain expertise (performance optimization for backend)
- Task-specific rules (test generation requirements)

**DON'T:**
- Generic "how to be helpful" guidance (too broad)
- Context that belongs in `.github/copilot-instructions.md` (workspace-wide)
- Duplicate instructions across multiple files (consolidate)
- Tool references without agent scope clarification

---

## Part 2: YAML Frontmatter

Instructions files use optional YAML frontmatter to configure scope and metadata.

### Required/Recommended Fields

#### `applyTo` (Conditional; Required if Selective Application)

Glob pattern defining which files receive these instructions.

**DO:**
```yaml
applyTo: "**/*.py"                    # All Python files
applyTo: "src/backend/**"            # Backend folder only
applyTo: "**/*.test.js"              # Test files only
applyTo: "**/*.md,**/*.mdx"           # Markdown and MDX
```

**DON'T:**
```yaml
applyTo: "**/*"                      # Too broad; use .github/copilot-instructions.md instead
applyTo: "*.{js,ts,jsx,tsx}"         # Missing **/ for recursive matching
```

#### `name` (Optional)

Human-readable name for UI display (defaults to filename).

**DO:**
```yaml
name: Python Code Standards
```

#### `description` (Optional)

Brief summary of instruction purpose.

**DO:**
```yaml
description: PEP 8 compliance and Python best practices for project consistency
```

### Frontmatter Example

```yaml
---
name: Backend Security Review Guidelines
description: Security, performance, and data integrity checks for backend services
applyTo: "src/backend/**/*.{js,ts}"
---
```

---

## Part 3: Instruction File Body

The body contains the actual instructions. Use Markdown formatting with clear structure.

### Structure Guidelines

#### 1. Lead Statement (Required)

Open with imperative purpose statement.

**DO:**
```markdown
Follow PEP 8 style guide for all Python code.
Prioritize readability and clarity in implementation.
Use type hints for all function signatures.
```

**DON'T:**
```markdown
This file contains guidelines for Python development.
You should consider following best practices.
```

#### 2. Organized Sections (Optional but Recommended)

Group related instructions by concern.

**DO:**
```markdown
## Code Style
- Use 4-space indentation (never tabs)
- Maximum line length: 100 characters
- Use snake_case for variables and functions

## Documentation
- Write docstrings for all functions
- Include type hints: `def process(data: list[dict]) -> str:`
- Document exceptions explicitly

## Testing
- Write unit tests for all functions
- Maintain >80% code coverage
- Name tests: `test_<function_name>_<scenario>`

## Performance
- Avoid N+1 database queries; use batch loading
- Optimize hot paths with profiling
- Cache frequently accessed data
```

#### 3. Specificity (Required)

Avoid vague guidance. Be concrete and actionable.

**DO:**
```markdown
- Use pytest for unit testing, not unittest
- Write fixtures in conftest.py for reusability
- Use `pytest -v --cov` for coverage reports
```

**DON'T:**
```markdown
- Write good tests
- Make sure code is well-tested
- Tests should be comprehensive
```

#### 4. Tool References (Optional)

Reference tools using `#tool:<tool-name>` syntax.

**DO:**
```markdown
When analyzing code, use #tool:usages to find symbol references.
Verify compatibility with #tool:githubRepo before implementing.
```

#### 5. Exceptions & Edge Cases (Optional)

Clarify when rules don't apply.

**DO:**
```markdown
## Exceptions

- **Third-party code:** Don't enforce style rules on vendored dependencies
- **Generated files:** Exempt `*.pb.py` (protobuf) and `*.g.py` (generated) from style checks
- **Legacy code:** Exemptions require explicit approval from tech lead
```

### Example: Complete Instructions File

```markdown
---
name: TypeScript Backend Standards
description: Code style, security, and best practices for TypeScript backend services
applyTo: "src/backend/**/*.ts"
---

## Imperative Style

Prioritize clarity, security, and maintainability in all TypeScript backend code.

## Code Style

- Use ESLint configuration from `.eslintrc.json`
- Enforce strict mode: `"strict": true` in `tsconfig.json`
- Use PascalCase for classes and types; camelCase for functions/variables
- Maximum line length: 100 characters
- Use semicolons; omit trailing commas in multi-line structures
- Indent with 2 spaces

## Type Safety

- Use strict TypeScript settings: `noImplicitAny: true`, `strictNullChecks: true`
- Never use `any` type; use `unknown` with type guards instead
- Export explicit return types for all functions
- Use `readonly` for immutable object properties

## Security

- Sanitize all user inputs before database queries (use parameterized queries)
- Validate request payloads against schema (use Zod or Joi)
- Do NOT log sensitive data (passwords, tokens, API keys)
- Enforce HTTPS-only for all API endpoints
- Implement rate limiting on public endpoints (#tool:usages)

## Error Handling

- Use typed error classes extending `Error`
- Never swallow exceptions; log or re-throw with context
- Return structured error responses: `{ code, message, details }`
- Document expected exceptions in JSDoc comments

## Testing

- Write unit tests in `*.test.ts` files co-located with source
- Use Jest with 80%+ code coverage threshold
- Test error cases explicitly; happy path only is insufficient
- Mock external dependencies (databases, APIs, file systems)

## Performance

- Use database indexes for frequently queried columns
- Implement query result caching (Redis) for expensive operations
- Profile hot paths with Node's built-in profiler before optimizing
- Log execution time for API endpoints >100ms

## Exceptions

- **Third-party types:** Use `@ts-ignore` only when types are unavailable
- **Legacy integrations:** Security rules cannot be exempted; styling exemptions require tech lead approval
```

---

## Part 4: Best Practices

### Single Responsibility

Each `.instructions.md` file should address one concern or domain.

**DO:**
```
python-style.instructions.md     # Python-specific style
security-review.instructions.md  # Security checks (language-agnostic)
react-patterns.instructions.md   # React component patterns
```

**DON'T:**
```
general-coding.instructions.md   # Mix of everything; too broad
```

### Reusability

Instructions files are applied to chat requests automatically. Design for multiple contexts.

**DO:**
```markdown
- Short, self-contained statements (one idea per line)
- Reference external docs for complex topics
- Use tool references for context-aware guidance
```

**DON'T:**
```markdown
- Long paragraphs with multiple concepts
- Instructions that only work in specific chat contexts
```

### Glob Pattern Precision

Use glob patterns to target exact scope; avoid over-application.

**DO:**
```yaml
applyTo: "src/backend/**/*.ts"          # Specific: backend TypeScript only
applyTo: "**/*.test.{js,ts}"            # Specific: test files only
applyTo: "**/components/**/*.{vue,jsx}" # Specific: component files
```

**DON'T:**
```yaml
applyTo: "**/*"                         # Too broad; makes file universal
applyTo: "src/**"                       # Applies to entire src; consider narrower
```

### Length & Complexity

Keep instructions concise and scannable.

**DO:**
- 5-20 focused guidance items per file
- Bullet points or short sections
- Assume reader is familiar with the domain

**DON'T:**
- 50+ items (split into multiple files)
- Long narrative paragraphs
- Repetition across multiple files

---

## Part 5: Scope & Application

### Workspace vs. User Profile

**Workspace Instructions** (`.github/instructions/*.md`):
- Shared with team via version control
- Project-specific or domain-specific
- Applied only within that workspace
- Recommended for team consistency

**User Instructions** (User profile):
- Personal preferences across workspaces
- Not shared with team
- Applied in all workspaces
- Use for personal coding style

**Recommendation:** Store team standards in workspace; personal preferences in user profile.

### Glob Pattern Examples

| Pattern | Matches |
|---------|---------|
| `**/*.py` | All Python files in workspace |
| `src/**/*.ts` | TypeScript files in src folder |
| `**/*.test.js` | Jest test files |
| `**/components/**/*.{jsx,tsx}` | React component files |
| `docs/**/*.md` | Markdown documentation |
| `**/{api,server}/**` | API or server folders |

---

## Part 6: Integration with Other File Types

### Reference Instructions in Agent Files

Agent files (`.agent.md`) can reference instructions:

```markdown
---
description: Code implementation agent
tools: ['create_file', 'replace_string_in_file']
applyTo: "src/**"
---

You are the Implementation Agent. Generate code following:
- [Python standards](../instructions/python-style.instructions.md)
- [Security guidelines](../instructions/security-review.instructions.md)
```

### Reference Instructions in Prompt Files

Prompt files (`.prompt.md`) can reference instructions:

```yaml
---
agent: implementation
instructions: ["../instructions/python-style.instructions.md"]
---

Implement this feature following best practices.
```

---

## Part 7: Compliance Checklist

Verify instructions file before committing:

- [ ] **Scope Clear:** File purpose is single-concern, not multi-domain
- [ ] **Glob Pattern Precise:** `applyTo` targets intended files, not over-broad
- [ ] **Frontmatter Complete:** name, description, applyTo provided (if selective)
- [ ] **Body Structured:** Sections organized by concern (Style, Security, Testing, etc.)
- [ ] **Language Imperative:** Follows communication style standard
  - [ ] No "please," "would you," "could you"
  - [ ] Imperative verbs ("Use...", "Write...", "Do NOT...")
  - [ ] Direct constraints ("Do NOT use...", not "Avoid...")
  - [ ] No hedging ("arguably", "perhaps")
  - [ ] No emojis
- [ ] **Specificity:** Guidance is concrete and actionable
- [ ] **Conciseness:** 5-20 items; <1000 words
- [ ] **No Duplication:** Same guidance not repeated across files
- [ ] **Tool References:** `#tool:<name>` syntax used correctly (if applicable)
- [ ] **Examples Provided:** Complex concepts include concrete examples
- [ ] **Exceptions Listed:** Edge cases or exemptions documented

---

## Part 8: Common Instruction Domains

### Language-Specific Standards

**Python (`*.py`):**
```markdown
Follow PEP 8 style guide.
Use type hints for all function signatures.
Write docstrings for all functions/classes.
Use pytest for testing.
Maintain >80% code coverage.
```

**TypeScript/JavaScript (`*.ts`, `*.js`):**
```markdown
Use ESLint configuration from project.
Enforce strict mode.
Use PascalCase for classes; camelCase for functions.
Write JSDoc comments for exported functions.
Use Jest for unit testing.
```

**Go (`*.go`):**
```markdown
Follow Go conventions: CamelCase for exports, camelCase for private.
Use error handling explicitly (no try-catch).
Format with gofmt.
Write tests in `*_test.go` files.
Document exported functions with GoDoc.
```

### Domain-Specific Standards

**Security Review:**
```markdown
Validate all user inputs; use parameterized queries.
Do NOT log sensitive data (passwords, tokens, keys).
Enforce HTTPS-only for APIs.
Implement rate limiting on public endpoints.
Use security headers (CSP, X-Frame-Options, etc.).
```

**Performance Optimization:**
```markdown
Profile before optimizing; use profiler data to guide.
Avoid N+1 database queries; use batch loading.
Cache frequently accessed data (Redis, memoization).
Lazy-load large dependencies.
Monitor metric: P95 latency, memory usage, CPU usage.
```

**Documentation:**
```markdown
Write in active voice; address reader directly.
Use code examples for complex concepts.
Keep examples <20 lines; reference full examples elsewhere.
Update docs with code changes (same PR).
Use consistent terminology across docs.
```

### Testing Standards

```markdown
Write unit tests for all public functions.
Test error cases explicitly; happy path only is insufficient.
Use fixtures for setup/teardown; avoid repeated code.
Mock external dependencies (APIs, databases, file systems).
Maintain >80% code coverage; >90% for critical paths.
Name tests descriptively: test_<function>_<scenario>.
```

---

## Part 9: File Organization

### Directory Structure

```
.github/
├── instructions/
│   ├── python-style.instructions.md
│   ├── typescript-style.instructions.md
│   ├── security-review.instructions.md
│   ├── performance-optimization.instructions.md
│   ├── testing-standards.instructions.md
│   └── documentation.instructions.md
├── agents/
│   ├── implementation.agent.md
│   └── code-review.agent.md
└── copilot-instructions.md          # Workspace-wide fallback
```

### Naming Convention

Instructions files follow pattern: `{name}.{domain}.instructions.md`

Example: `python-style.core.instructions.md`, `security-review.core.instructions.md`

---

## Examples of Well-Formed Instructions Files

### Example 1: Python Style Guide

```markdown
---
name: Python Code Standards
description: PEP 8 compliance and best practices for Python code
applyTo: "**/*.py"
---

Follow PEP 8 style guide for all Python code.

## Code Style

- Use 4-space indentation (never tabs)
- Maximum line length: 100 characters
- Use snake_case for variables and functions; PascalCase for classes
- Use `# type: ignore` comments only when type hints are unavailable

## Type Hints

- Add type hints to all function signatures
- Use `from typing import ...` for complex types
- Document return types explicitly

## Documentation

- Write docstrings for all functions, classes, and modules
- Use docstring format: description, args, returns, raises
- Include examples for complex functions

## Testing

- Write unit tests in `tests/` directory
- Use pytest; configure in `pytest.ini`
- Maintain >80% code coverage
- Name tests: `test_<module>_<function>_<scenario>.py`
```

### Example 2: Security Review Guidelines

```markdown
---
name: Security Review Checklist
description: Security, data integrity, and privacy checks for all code
applyTo: "**/*.{js,ts,py,go,java}"
---

Perform security review on all code changes.

## Input Validation

- Validate all user inputs; never trust client data
- Use schema validation: Zod (JS/TS), Pydantic (Python), protobuf
- Sanitize inputs for database queries; use parameterized queries
- Reject oversized payloads; implement request size limits

## Secrets & Credentials

- Do NOT hardcode secrets, API keys, or passwords
- Use environment variables for credentials
- Do NOT log sensitive data (authentication tokens, PII)
- Rotate secrets regularly; document rotation process

## Authentication & Authorization

- Implement role-based access control (RBAC)
- Validate user permissions for every protected operation
- Use secure session management (HTTPOnly cookies, CSRF tokens)
- Implement rate limiting on authentication endpoints

## Data Privacy

- Encrypt sensitive data at rest (database, file system)
- Encrypt data in transit (HTTPS, TLS 1.3+)
- Implement data retention policies; delete PII when no longer needed
- Log data access for audit trails

## Dependencies

- Use `npm audit` (JavaScript) / `pip audit` (Python) / `go mod tidy` (Go)
- Update dependencies regularly; patch security vulnerabilities immediately
- Do NOT use unmaintained or untrusted packages
- Pin versions in lock files; review updates before merging
```

### Example 3: React Component Standards

```markdown
---
name: React Component Patterns
description: Component structure, hooks, and best practices for React
applyTo: "src/**/*.{jsx,tsx}"
---

Build React components following these patterns.

## Component Structure

- Use functional components and hooks; avoid class components
- Co-locate component, styles, and tests in same folder
- Export component as default; export types as named exports
- Use TypeScript for all props and state types

## Props & State

- Use TypeScript interfaces for prop validation
- Destructure props in function signature
- Avoid prop drilling; use Context API or state management for deep props
- Use `React.memo()` for expensive computations

## Hooks

- Use built-in hooks: `useState`, `useEffect`, `useCallback`, `useMemo`
- Use custom hooks to share component logic
- Place hook calls at component top level (never conditional)
- Clean up side effects in `useEffect` dependencies

## Testing

- Test components with React Testing Library
- Test user interactions, not implementation details
- Use `data-testid` sparingly; prefer semantic queries
- Maintain >80% coverage for components
```

---

## Tips for Success

1. **Keep focused:** One domain per file; avoid mixing concerns
2. **Be specific:** Generic guidance helps less than concrete rules
3. **Reference, don't duplicate:** Link to other instruction files; don't repeat
4. **Version control:** Store in `.github/instructions/` for team sharing
5. **Review regularly:** Update instructions when practices evolve
6. **Test application:** Verify glob patterns match intended files
7. **Document exceptions:** Explain when rules don't apply

---

## Troubleshooting

**Instructions not applying:**
- Verify `applyTo` glob pattern matches file paths
- Check file is in `.github/instructions/` (workspace) or user profile
- Reload VS Code chat context

**Too many instructions applying:**
- Make glob patterns more specific
- Split broad files into multiple focused files
- Check for overlapping `applyTo` patterns

**Instructions ignored in AI response:**
- Verify instructions file exists and is readable
- Check for syntax errors in YAML frontmatter
- Confirm instructions are semantically clear (not contradictory)
