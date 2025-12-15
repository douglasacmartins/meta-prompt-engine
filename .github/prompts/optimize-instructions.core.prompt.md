---
name: Optimize Instructions System
description: Audit instructions files for completeness, consistency, and effectiveness
argument-hint: "Analyze instruction files for optimization opportunities"
agent: master-planner
---

# Optimize Instructions System

Audit the instructions system for gaps, inconsistencies, and optimization opportunities.

## Analysis Scope

Review all instructions files in `.github/instructions/`:
- `communication-style.vscode.instructions.md` — Communication style rules
- `file-naming-convention.vscode.instructions.md` — Naming patterns
- `design-standards.design.instructions.md` — Design file standards
- `agent.design.instructions.md` — Agent file specifications
- `instructions.design.instructions.md` — Instructions file specifications
- `prompt.design.instructions.md` — Prompt file specifications

## Checks to Perform

**Completeness:**
- Are all required frontmatter fields documented for each file type?
- Do all design files have compliance checklists?
- Are all file structures fully explained?
- Are examples provided for complex concepts?

**Consistency:**
- Do all files follow imperative, direct communication style?
- Are naming conventions applied consistently across examples?
- Are `applyTo` patterns correct and non-overlapping?
- Do all files use consistent section structure?

**Duplication:**
- Are there any repeated examples or explanations across files?
- Do any two files cover the same topic redundantly?
- Are guidelines duplicated instead of referenced?

**Clarity:**
- Are sections clearly organized with logical hierarchy?
- Are examples concrete and actionable?
- Do guidelines avoid vague language?
- Are constraints explicit ("Do NOT...", not "Avoid...")?

## Validation Checklist

- **Completeness:**
  - Are all required frontmatter fields documented?
  - Do all design files have compliance checklists?
  - Are examples provided for complex concepts?

- **Consistency:**
  - Do all files follow imperative, direct communication style?
  - Are naming conventions applied consistently?
  - Do all files use consistent section structure?

- **Clarity:**
  - Are sections clearly organized with logical hierarchy?
  - Are examples concrete and actionable?
  - Do guidelines avoid vague language?

- **Duplication:**
  - Are there any repeated examples or explanations across files?
  - Do any two files cover the same topic redundantly?

## Output Format

Provide structured analysis:

**Issues Found:**
- Category: [Completeness|Consistency|Duplication|Clarity]
- File: [filename]
- Issue: [description]
- Location: [section/line]
- Severity: [Critical|High|Medium|Low]

**Recommendations:**
- [Action to take]
- [Impact of change]

**Summary:**
- Total issues by category
- Priority fixes needed
- Overall health assessment

## Standards Reference

All instructions must comply with:
- Imperative mood (commands, not requests)
- No politeness markers ("please", "could you", "would you")
- Direct constraints ("Do NOT...", not "Avoid...")
- No hedging language ("arguably", "perhaps", "seems")
- No emojis (use DONE/READY/WARNING/ERROR)
- Specific, actionable guidance (not vague)
- No duplication across files
- Proper `applyTo` patterns (no overlap)

## Guidelines

- Flag missing or incomplete specifications
- Identify vague or ambiguous language
- Check for contradictions between files
- Verify `applyTo` patterns don't conflict
- Ensure examples are current and accurate
- Check that compliance checklists are comprehensive
