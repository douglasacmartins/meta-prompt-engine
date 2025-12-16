---
name: Design Standards for Customization Files
description: Shared standards and compliance guidelines for agent, instructions, and prompt design files
applyTo: ".github/**/*.agent.md, .github/**/*.instructions.md, .github/**/*.prompt.md"
---

# Design Standards for Customization Files

This file establishes shared standards that apply to all design specification files (agent, instructions, and prompt design files). Use these guidelines when creating or reviewing design standards.

---

## Shared Overview

All customization files in VS Code fall into three categories:

| Type | Scope | Trigger | Reusability |
|------|-------|---------|-------------|
| **Agents** (`*.agent.md`) | Define personas with specialized tools | Manual or automatic (handoff) | Standalone or via handoff |
| **Instructions** (`*.instructions.md`) | Apply to file types/folders | Automatic (glob pattern) | Referenced by agents/prompts |
| **Prompts** (`*.prompt.md`) | Define single-task workflows | Manual (`/prompt-name` in chat) | Standalone, on-demand |

---

## Required Frontmatter Fields

All customization file design specifications must document frontmatter requirements for their file type.

### No Universal Fields

Each file type (agent, instructions, prompt) has completely distinct frontmatter requirements:

- **Agents** (`*.agent.md`): description, name, tools, argument-hint, model, target, infer, mcp-servers, handoffs
- **Instructions** (`*.instructions.md`): applyTo, name, description
- **Prompts** (`*.prompt.md`): name, description, argument-hint, agent, tools, model

Even fields that appear in multiple files (e.g., `name`, `description`) have type-specific meanings and usage patterns.

### Type-Specific Fields

Each design file documents its complete frontmatter specification. Do NOT duplicate across design files; keep specialization per file type. Do NOT assume fields are "universal" — each file type is independent.

---

## No Cross-File References

Each file is self-contained. Do NOT reference other instruction files via markdown links. Scope is defined solely by `applyTo` glob patterns. VS Code automatically composes applicable instructions based on these patterns—manual linking creates duplication and maintenance burden.

**Forbidden:**
```markdown
See: File Name -> ../path/file.md
Reference: Communication Style -> communication-style.vscode.instructions.md
```

Communication style, naming conventions, and other shared standards are enforced by separate instruction files that apply automatically via their `applyTo` patterns. Trust the composition system.

---

## Shared Structure Guidelines

## Shared Structure Guidelines

All design files should follow this organization:

1. **YAML Frontmatter** — Metadata (name, description, applyTo)
2. **Main Heading** — File title matching name field
3. **Brief Introduction** — 1-2 sentence description
4. **Horizontal Rule** — `---` separator
5. **Numbered Parts** — Logical sections (Part 1, Part 2, etc.)
6. **Compliance Checklist** — Verification steps before commit
7. **Related Standards** — Links to cross-referenced files
8. **Examples/Appendix** — Concrete demonstrations (optional)

---

## Frontmatter Specificity Principle

Each design file documents ONLY its type-specific frontmatter fields.

**DO NOT duplicate** frontmatter guidance across files:
- Agent design documents agent-specific fields (tools, handoffs, target, infer, mcp-servers)
- Instructions design documents instructions-specific fields (applyTo, name, description)
- Prompt design documents prompt-specific fields (name, description, argument-hint, agent, tools, model)

**Universal fields** (name, description) can be mentioned, but focus on type-specific requirements.

---

## DRY Principle for Design Files

Avoid repeating content across design specification files:

### KEEP in Each File
- Type-specific frontmatter fields
- Type-specific body structure and sections
- Type-specific compliance checklist items
- Type-specific examples and patterns

### REMOVE from Design Files (Already Defined Elsewhere)
- Communication style rules (communication-style.vscode.instructions.md)
- File naming patterns (file-naming-convention.vscode.instructions.md)
- Tool reference tables (tool-usage.design.instructions.md)
- Shared Overview table (include in agent-design only; reference from other two)

### How to Reference Instead of Duplicate

**Style rules:**
```markdown
Follow the Communication Style Standard (communication-style.vscode.instructions.md):
- Imperative mood, not requests
- Direct constraints ("Do NOT...")
- No hedging language
- No emojis; use explicit status markers
```

**Naming patterns:**
```markdown
Naming follows the File Naming Convention Standard (file-naming-convention.vscode.instructions.md) pattern: `{name}.{domain}.{type}.md`
```

**Tool reference:**
```markdown
See tool-usage.design.instructions.md for complete tool listing.
```

---

## Compliance Checklist for Design Files

Before committing any design specification file, verify:

### General Compliance
- [ ] **Frontmatter Valid:** YAML frontmatter is syntactically correct
- [ ] **applyTo Scope:** Glob pattern targets intended file types
- [ ] **Title Matches Name:** Main heading matches `name` field
- [ ] **Structured:** Uses numbered Parts or clear sections
- [ ] **File Naming:** Uses pattern `{name}.design.instructions.md`

### Communication & Language
- [ ] **Imperative Style:** All instructions use commands
- [ ] **No Politeness:** No "please," "could you," "would you"
- [ ] **Active Voice:** Direct responsibility assignment
- [ ] **Direct Constraints:** Uses "Do NOT", "Must", "Never"
- [ ] **No Hedging:** No "arguably," "perhaps," "seems like"
- [ ] **No Emojis:** Uses DONE/WARNING/ERROR instead
- [ ] **Consistent Tone:** Professional, direct, authoritative

### Content Quality
- [ ] **Specificity:** Examples are concrete, not vague
- [ ] **Completeness:** Covers required and optional fields/sections
- [ ] **No Duplication:** Doesn't repeat content from other design files
- [ ] **Clear Examples:** Demonstrates DO/DON'T patterns
- [ ] **Organized:** Uses consistent section hierarchy

### References & Links
- [ ] **Cross-References:** Refer to related standards by filename (no markdown links)
- [ ] **Communication Style:** References communication-style.vscode.instructions.md
- [ ] **Naming Convention:** References file-naming-convention.vscode.instructions.md
- [ ] **Type-Specific:** References type-specific examples by filename (no markdown links)

### Frontmatter Compliance
- [ ] **Unique Focus:** Addresses one file type (agent / instructions / prompt)
- [ ] **Specific Fields:** Documents type-specific frontmatter requirements
- [ ] **Required/Optional:** Marks fields as required, recommended, or optional
- [ ] **Constraints:** Explains when and why to use each field

### Structure Compliance
- [ ] **Introduction:** Brief purpose statement (1-2 sentences)
- [ ] **Numbered Parts:** Logical organization with numbered sections
- [ ] **Body Structure:** Explains how to structure Markdown content
- [ ] **Examples:** Includes DO/DON'T examples for each major concept
- [ ] **Compliance Checklist:** Provides verification steps
- [ ] **File Location:** Documents where files should be stored

---

## Design File Organization

### File Storage

```
.github/instructions/
├── agent.design.instructions.md
├── instructions.design.instructions.md
├── prompt.design.instructions.md
└── design-standards.design.instructions.md  # This file
```

### File Purposes

- **agent.design.instructions.md** — How to create `.agent.md` files
- **instructions.design.instructions.md** — How to create `.instructions.md` files
- **prompt.design.instructions.md** — How to create `.prompt.md` files
- **design-standards.design.instructions.md** — Shared standards for all design files (this file)
