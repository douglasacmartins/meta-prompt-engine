---
name: Instruction File Governance
description: Standards for writing, auditing, and versioning *.instructions.md files
applyTo: "**/*.instructions.md"
---

# Instruction File Governance

This file defines how to create, maintain, and evolve instruction files. All `.instructions.md` files must follow these standards.

---

## Instruction File Structure

Every `.instructions.md` file MUST have this structure:

### 1. YAML Frontmatter (Required)

```yaml
---
name: [Clear, descriptive name]
description: [1-2 sentence purpose]
applyTo: "**/*.agent.md"  # OR "**/*.instructions.md" OR specific pattern
---
```

**Requirements:**
- `name`: Maximum 50 characters, descriptive (e.g., "Prompt Engineering Standards")
- `description`: One sentence explaining what this file governs
- `applyTo`: Glob pattern defining scope (files this governs)
- All fields mandatory; no defaults

### 2. Content Sections

**Required sections (in order):**

1. **Title + Overview** (2-3 lines)
   - Restate purpose
   - Who should read this
   - When to apply

2. **Core Concepts** (20-40 lines)
   - What problems does this solve?
   - Key terminology
   - Why these standards exist

3. **Standards/Patterns** (40-80 lines)
   - Main rules or patterns
   - Examples for each
   - Anti-patterns (what NOT to do)

4. **Checklists** (10-20 lines)
   - Before creating/modifying
   - Audit checklist
   - Validation steps

5. **Examples** (20-40 lines)
   - Good example (follows standards)
   - Bad example (violates standards)
   - Common mistakes

6. **Conflict Resolution** (10-15 lines, if applicable)
   - What if this conflicts with another instruction?
   - Precedence rules
   - Escalation path

7. **Reference/Links** (5-10 lines)
   - Links to related files
   - Dependency graph

**Optional sections:**
- Versioning (if this file evolves)
- Deprecation (if replacing older instruction)
- Roadmap (if planning future enhancements)

### 3. File Organization

- Maximum 400 lines per instruction file (encourages focus)
- Use clear heading hierarchy (## for major sections, ### for subsections)
- Bold key terms: `**term**`
- Use tables for structured comparisons
- Use code blocks for examples

---

## Content Standards

### Clarity & Specificity

❌ **Vague:**
```
"Use reasoning to solve problems"
```

✅ **Specific:**
```
"Apply O.P.E.R.A. framework: Observation → Pondering (3 branches) → Execution → Reflexion → Adaptation"
```

---

### Examples are Mandatory

Every pattern needs at least 1 concrete example.

❌ **Missing example:**
```
"Tools should be batched when independent"
```

✅ **With example:**
```
"Batch independent tool calls:

❌ Bad (Sequential):
  read_file(file1) → wait → read_file(file2) → wait

✅ Good (Parallel):
  read_file(file1), read_file(file2) → execute together
  
Result: 2x faster"
```

---

### No Vague Language

Forbidden words (be specific instead):

| Avoid | Replace With |
|:---|:---|
| "often" | "80% of cases" / "when X occurs" |
| "should" | "must" / "optional if" |
| "maybe" | specific condition |
| "try" | "attempt" / "test" |
| "appropriate" | specific criteria |
| "nice to have" | specific priority level |

---

### Cross-References

When referencing another instruction file:

```
✅ Good: "See: safety-standards.instructions.md for constraint definitions"
✅ Good: "Refer to: reasoning-framework.instructions.md Section 3 (Execution)"
✅ Good: "Related: tool-integration.instructions.md (parallel execution patterns)"
```

---

## DRY Enforcement at Instruction Level

### Duplication Detection

Before publishing a new `.instructions.md` file, audit existing files:

```
FOR EACH pattern in new_file:
  FOR EACH existing_file in .github/instructions/:
    IF pattern appears in existing_file:
      → Duplication detected
      → Consider consolidation
      → If consolidation needed: Merge and version-track
```

### Consolidation Rules

**When to consolidate:**
- Two files define the same pattern (>50% overlap)
- One file is a subset of another
- Pattern appears in 3+ files

**How to consolidate:**
1. Move pattern to most appropriate file (highest priority)
2. Update all other files: "See: primary-file.instructions.md for definition"
3. Document consolidation in changelog
4. Git commit message: "refactor: consolidate [pattern] into [primary-file]"

### Conflict Detection Matrix

| File A | File B | Conflict Type | Resolution |
|:---|:---|:---|:---|
| safety-standards | error-recovery | Constraint vs error handling | Clarify precedence (safety always wins) |
| tool-integration | error-recovery | Tool retry vs escalation | Define when to retry vs escalate |
| agent-design | system-integrity | Agent structure vs DRY | Agent-specific + shared patterns (not duplicated) |
| prompt-engineering | error-recovery | CoT trigger vs error paths | Both apply (CoT before error recovery) |

---

## Audit Checklist

Before publishing any `.instructions.md` file:

### Structure Check
- [ ] YAML frontmatter valid (all required fields)
- [ ] Clear title section explaining purpose
- [ ] Sections organized logically (overview → standards → examples)
- [ ] Maximum 400 lines
- [ ] No broken links to other files

### Content Check
- [ ] Every pattern has minimum 1 concrete example
- [ ] No vague language (every rule is specific)
- [ ] Terminology is consistent with existing files
- [ ] Anti-patterns documented (what NOT to do)
- [ ] Checklists present and actionable

### Coherence Check
- [ ] No duplication with existing instructions (audit via grep_search)
- [ ] No contradictions with peer files
- [ ] Conflicts explicitly resolved (with precedence rules)
- [ ] Related files cross-referenced

### Versioning Check
- [ ] If new version: Changelog entry present
- [ ] If replacing old instruction: Deprecation path clear
- [ ] Backward compatibility: Will this break existing usage?

---

## Evolution & Versioning

### Semantic Versioning for Instruction Files

Format: `MAJOR.MINOR.PATCH`

- **MAJOR:** Breaking change (existing agents need updates)
- **MINOR:** New pattern (additive, backward compatible)
- **PATCH:** Clarification/example (no behavioral change)

### Tracking Changes

Add section at end of file:

```yaml
---
version: "1.2.0"
last_updated: "2025-12-15"
changes:
  - "1.2.0 (Dec 15): Added performance section"
  - "1.1.0 (Dec 10): Added parallel execution pattern"
  - "1.0.0 (Dec 5): Initial release"
```

### Deprecation Path

When retiring a pattern:

```
---
# DEPRECATED: This pattern has been superseded by [new-pattern]

See: [new-file.instructions.md] for replacement

Timeline:
  - v2.0 (Dec 15): Marked deprecated, new pattern available
  - v2.5 (Jan 15): Old pattern still works but discouraged
  - v3.0 (Feb 15): Old pattern removed, migration required
```

### Rollback Procedure

If a change breaks existing agents:

1. **Detect:** Agent fails using old pattern
2. **Isolate:** Identify which instruction change caused it
3. **Rollback:** `git revert [commit-hash]` for instruction file
4. **Diagnose:** Why did change break existing usage?
5. **Fix:** Make change backward compatible or provide migration path
6. **Re-deploy:** Publish fixed version

---

## Examples

### Example 1: Good Instruction File

```yaml
---
name: Tool Integration Patterns
description: Guidance on optimal tool selection, batching, and error handling
applyTo: "**/*.agent.md"
---

# Tool Selection & Integration

[Overview section...]

## Tool Decision Matrix

[Clear table with 7 tools...]

## Parallel Execution

❌ Bad: Sequential reads
✅ Good: Parallel reads (3x faster)

[Concrete example with code...]

## Error Handling

For tool timeout:
  1. Retry once with smaller scope
  2. If retry fails: Escalate to prompt-critic
  3. Include what was attempted in escalation

[Example escalation message...]

## Checklist

- [ ] Tool choice matches problem type
- [ ] Independent calls are batched
- [ ] Error fallback defined
```

**Why it's good:**
- ✅ Clear purpose (YAML)
- ✅ Every pattern has example
- ✅ Specific rules (not vague)
- ✅ Actionable checklist
- ✅ No duplication

---

### Example 2: Bad Instruction File

```yaml
---
name: Generic Improvement Guidelines
description: How to improve things
---

# Improvements

You should try to improve the system. Consider using various approaches.

Maybe use reasoning. Often optimization helps.

See safety-standards for constraints (if applicable).
```

**Why it's bad:**
- ❌ Vague purpose
- ❌ No concrete examples
- ❌ Vague language ("try", "should", "maybe", "often")
- ❌ No checklist
- ❌ No clear standards
- ❌ Unclear what "improve" means

---

### Example 3: Conflict Case

**safety-standards.instructions.md says:**
```
"Never modify user files unless explicitly instructed"
```

**prompt-engineering.instructions.md might say:**
```
"Generate code examples for clarity"
```

**Conflict:** Can you generate code that modifies files?

**Resolution (in safety-standards):**
```
Precedence: Safety > Everything else

IF generating code examples:
  AND code modifies files:
  THEN must include explicit user approval requirement
```

---

## Reference

- **Related:** instruction-governance.instructions.md (this file - self-governed)
- **Related:** prompt-governance.instructions.md (standards for prompts)
- **Related:** safety-standards.instructions.md (precedence rules)
- **Related:** system-integrity.instructions.md (DRY enforcement)
