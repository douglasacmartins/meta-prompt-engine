---
name: File Naming Convention Standard
description: Enforce consistent naming pattern {name}.{domain}.{type}.md for agents, instructions, and prompts
applyTo: ".github/**/*.agent.md,.github/**/*.instructions.md,.github/**/*.prompt.md"
---

# File Naming Convention Standard

All customization files (agents, instructions, prompts) must follow the naming pattern: `{name}.{domain}.{type}.md`

This standard ensures consistency, discoverability, and clear domain organization across the entire ecosystem.

---

## Naming Pattern

### Pattern

```
{name}.{domain}.{type}.md
```

Where:
- **{name}** — File purpose or role; kebab-case
- **{domain}** — Specialization or context; kebab-case
- **{type}** — File category: `agent`, `instructions`, or `prompt`

### Components

#### {name} — File Purpose

Describes what the file does. Use imperative verbs or nouns.

**Agents:**
```
logic-validator.reasoning.agent.md
code-analyzer.vscode.agent.md
security-reviewer.core.agent.md
```

**Instructions:**
```
typescript-style.code-standards.instructions.md
security-review.guidelines.instructions.md
api-documentation.standards.instructions.md
```

**Prompts:**
```
generate-form.vscode.prompt.md
explain-code.analysis.prompt.md
write-tests.quality.prompt.md
```

#### {domain} — Specialization Context

Reflects where or how the file is used. Common domains:

**Core Domains:**
- `core` — Central/foundational
- `reasoning` — Logic and validation
- `infrastructure` — System/ops
- `workflow` — Multi-step processes

**Technology Domains:**
- `vscode` — VS Code specific
- `backend` — Backend services
- `frontend` — Frontend applications
- `python` — Python language
- `typescript` — TypeScript language

**Feature Domains:**
- `security` — Security concerns
- `performance` — Performance optimization
- `testing` — Test generation/review
- `documentation` — Doc generation

**Custom Domains:**
- Project-specific: `billing`, `payments`, `auth`, etc.

#### {type} — File Category

One of three values:
- `agent` — Agent definition file (`.agent.md`)
- `instructions` — Instructions file (`.instructions.md`)
- `prompt` — Prompt file (`.prompt.md`)

---

## Naming Conventions

### Name Component Rules

**DO:**
- Use kebab-case (lowercase, hyphens): `logic-validator`, `code-analyzer`
- Be specific: `security-reviewer` (not `reviewer`)
- Use imperative verbs for actions: `generate-form`, `write-tests`, `explain-code`
- Keep concise: 2-4 words maximum: `extension-developer` ✓, `vs-code-extension-developer` ✗

**DON'T:**
- Use underscores: `logic_validator` ✗
- Use spaces: `logic validator` ✗
- Use CamelCase: `LogicValidator` ✗
- Use redundant suffixes: `logic-agent` (redundant with `.agent.md`) ✗
- Be too generic: `helper`, `tool`, `utility` ✗

### Domain Component Rules

**DO:**
- Use kebab-case: `code-standards`, `security-review`
- Match actual context: If used across projects, use generic domain (`core`); if project-specific, use project name
- Align with folder structure (optional but recommended)
- Reuse consistent domains across related files

**DON'T:**
- Repeat name component: `logic-validator.logic.agent.md` ✗
- Use underscores: `code_standards` ✗
- Use overly broad domains: `stuff`, `misc` ✗

### Type Component Rules

**DO:**
- Use exact values: `agent`, `instructions`, `prompt`
- Match file extension: `.agent.md`, `.instructions.md`, `.prompt.md`

**DON'T:**
- Abbreviate: `agt`, `instr`, `prm` ✗
- Pluralize: `agents`, `prompts` ✗
- Vary: Consistency required across all files

---

## Examples by Category

### Agent Files

**Good:**
```
logic-validator.reasoning.agent.md        # Specific role, clear domain
code-analyzer.vscode.agent.md             # Feature-specific
security-reviewer.core.agent.md           # Core security role
extension-developer.vscode.agent.md       # VS Code specialization
```

**Bad:**
```
validator.agent.md                        # Too generic
logic_validator.reasoning.agent.md        # Wrong separator
logic-validator.agent.md                  # Missing domain
logic-validator-agent.reasoning.agent.md  # Redundant suffix
```

### Instructions Files

**Good:**
```
typescript-style.code-standards.instructions.md
security-review.guidelines.instructions.md
extension-api.vscode.instructions.md
python-style.code-standards.instructions.md
```

**Bad:**
```
style-guide.instructions.md                # Missing domain
typescript_style.instructions.md           # Wrong separator
style.typescript.instructions.md           # Reversed order
typescript-style.md                        # Missing type
```

### Prompt Files

**Good:**
```
generate-form.vscode.prompt.md
explain-code.analysis.prompt.md
write-unit-tests.testing.prompt.md
security-review.guidelines.prompt.md
refactor-to-async.typescript.prompt.md
```

**Bad:**
```
form-generator.prompt.md                   # Missing domain
generate_form.vscode.prompt.md             # Wrong separator
vscode.generate-form.prompt.md             # Reversed order
generate-form-for-vscode.prompt.md         # Domain too wordy
```

---

## Accepted Domain Organization

### Single Domain (Centralized)

All files must follow a single, centralized domain organization. This is the only accepted pattern.

**Standard Structure:**
```
.github/
├── agents/
│   ├── logic-validator.core.agent.md
│   ├── code-analyzer.core.agent.md
│   ├── extension-developer.vscode.agent.md
│   └── security-reviewer.core.agent.md
├── instructions/
│   ├── communication-style.core.instructions.md
│   ├── extension-api.vscode.instructions.md
│   ├── security-review.core.instructions.md
│   └── typescript-style.code-standards.instructions.md
└── prompts/
    ├── explain-code.core.prompt.md
    ├── generate-extension.vscode.prompt.md
    ├── security-review.guidelines.prompt.md
    └── write-tests.testing.prompt.md
```

**Organization Rules:**
- All agents stored in `.github/agents/`
- All instructions stored in `.github/instructions/`
- All prompts stored in `.github/prompts/`
- Domain suffix specifies specialization: `core`, `vscode`, `testing`, etc.
- No nested domain folders; flat structure only

**Domain Assignment:**
- Use `core` for foundational, workspace-wide files
- Use technology domains (`vscode`, `python`, `typescript`) for tech-specific files
- Use feature domains (`testing`, `security`, `performance`) for concern-specific files
- Use project domains (`auth`, `billing`, `payments`) for project-specific files

**NOT Accepted:**
- ✗ Multi-Domain (Specialized) — No `.github/domains/vscode/` structure
- ✗ Mixed Organization — No mixed folder organization
- ✗ Nested domain folders — All files at same level
- ✗ Multiple organization patterns — Consistency required

---

## Compliance Checklist

Verify file naming before committing:

- [ ] **Single Domain Enforced:** File uses domain consistent with workspace standard (e.g., all files use `.core.` or `.vscode.`)
- [ ] **No Nested Folders:** File stored in flat directory (`.github/agents/`, `.github/instructions/`, `.github/prompts/`), NOT in nested domain folder
- [ ] **Pattern Correct:** Matches `{name}.{domain}.{type}.md` exactly
- [ ] **{name} Component:**
  - [ ] Kebab-case (lowercase, hyphens only)
  - [ ] Specific and descriptive (2-4 words)
  - [ ] No redundant suffixes (`-agent`, `-instructions`, `-prompt`)
  - [ ] Reflects purpose or role
- [ ] **{domain} Component:**
  - [ ] Kebab-case
  - [ ] Matches workspace's chosen domain (e.g., `core`, `vscode`, `backend`)
  - [ ] Consistent with other files in workspace
  - [ ] Not empty or generic
- [ ] **{type} Component:**
  - [ ] Exact value: `agent`, `instructions`, or `prompt`
  - [ ] Lowercase
  - [ ] Matches file content
- [ ] **File Extension:**
  - [ ] `.agent.md` for agents
  - [ ] `.instructions.md` for instructions
  - [ ] `.prompt.md` for prompts
- [ ] **No Redundancy:**
  - [ ] Name doesn't repeat type (not `logic-agent.core.agent.md`)
  - [ ] Domain doesn't repeat name (not `logic-validator.validator.core.agent.md`)
- [ ] **Consistency:**
  - [ ] All other files in workspace use same domain approach
  - [ ] No typos or variations in domain usage
  - [ ] No nested domain folder structure

---

## Domain Selection Mandate

Choose a SINGLE domain for your entire workspace. Do NOT mix domains or create nested domain folders.

**Domain Selection Rules:**

1. **Choose ONE primary domain** for your ecosystem (e.g., `core`, `vscode`, `backend`, `payments`)
2. **Apply consistently** across ALL files: agents, instructions, prompts
3. **Secondary domains only for specialization**:
   - Use `core` for foundational, workspace-wide files
   - Use technology domains (`vscode`, `python`, `typescript`) when technology-specific
   - Use feature domains (`testing`, `security`, `performance`) for concern-specific files
   - Use project domains (`auth`, `billing`, `payments`) for project-specific files
4. **Never nest domains** — flat structure only in `.github/agents/`, `.github/instructions/`, `.github/prompts/`

**Example Selections:**

**Small Team (Foundational Focus):**
```
logic-validator.core.agent.md
communication-style.core.instructions.md
explain-code.core.prompt.md
security-review.core.instructions.md
write-tests.core.prompt.md
```
→ All files use `core` domain

**VS Code Extension Project (Technology Focus):**
```
extension-developer.vscode.agent.md
extension-api.vscode.instructions.md
generate-extension.vscode.prompt.md
communication-style.core.instructions.md  # Workspace-wide fallback
```
→ Primary domain is `vscode`; fallback to `core` for universal rules

**Backend Service (Feature Focus):**
```
code-analyzer.backend.agent.md
security-review.security.instructions.md  # Crosses concerns
performance-optimization.backend.instructions.md
write-tests.testing.prompt.md
```
→ Mix of `backend`, `security`, `testing` domains acceptable; NO nesting

---

## Renaming Examples

### Before → After (Single-Domain Correction)

| Before | After | Reason |
|--------|-------|--------|
| `validator.agent.md` | `logic-validator.core.agent.md` | Too generic; add domain |
| `security.instructions.md` | `security-audit.core.instructions.md` | Too vague; specify purpose and enforce `core` domain |
| `code_analyzer.py.agent.md` | `code-analyzer.python.agent.md` | Wrong separators; use consistent domain |
| `vscode-extension.vscode.prompt.md` | `generate-extension.vscode.prompt.md` | Too redundant; simplify name |
| `helper-functions.agent.md` | `utility-generator.core.agent.md` | Too generic; add domain |
| `.github/domains/vscode/agents/extension.vscode.agent.md` | `.github/agents/extension-developer.vscode.agent.md` | Remove nested domain folders; flatten structure |
| `.github/domains/security/instructions/audit.core.instructions.md` | `.github/instructions/security-audit.core.instructions.md` | Remove nested domain folders; all in flat `.github/instructions/` |

**Key Transformation Rules:**
- Remove nested domain folders (`.github/domains/vscode/` → `.github/agents/`)
- Flatten all files to single level within type directory
- Enforce consistent domain suffix across related files
- Simplify names; let domain suffix provide specialization context

---

## Related Standards

- [Agent Design Standard](agent-design.instructions.md) — Agent structure and frontmatter
- [Instructions File Design Standard](instructions-file-design.instructions.md) — Instructions structure
- [Prompt File Design Standard](prompt-file-design.instructions.md) — Prompt structure
- [Communication Style Standard](communication-style.instructions.md) — Language and tone

---

## Tips for Success

1. **Choose domain first:** Decide context/specialization, then name the file
2. **Keep names short:** Use shortest specific name; let domain provide context
3. **Be consistent:** Reuse domain names across related files
4. **Test discoverability:** File names should be guessable from purpose
5. **Avoid abbreviations:** Spell out domain and name fully
6. **Document domains:** List domains used in project README
