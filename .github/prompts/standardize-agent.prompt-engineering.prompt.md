---
name: standardize-agent-through-applicable-instructions
description: Guide agent to review and apply all applicable instruction standards before finalizing work
argument-hint: "Review customization file against applicable instruction standards"
agent: executor
---

# Standardize Agent Through Applicable Instructions

Ensure all customization files (agents, instructions, prompts) comply with applicable standards by systematically reviewing against applicable instruction files.

---

## Input

One or more customization files (`.agent.md`, `.instructions.md`, or `.prompt.md`) that need standardization review.

---

## Output

Structured compliance report with sections:
- **File Type Identified:** Agent, Instructions, or Prompt
- **Applicable Standards:** List of relevant instruction files
- **Compliance Checklist:** Status for each standard
- **Issues Found:** Non-compliant items with locations
- **Recommendations:** Specific changes needed for full compliance

---

## Task

Execute this workflow to standardize the customization file:

<instructions>

### Step 1: Identify File Type

Determine the customization file type from extension:
- `.agent.md` → Agent file
- `.instructions.md` → Instructions file  
- `.prompt.md` → Prompt file

### Step 2: Locate Applicable Standards

Find all instruction files that apply to this file type via `applyTo` glob patterns:

**All Files:**
- `file-naming-convention.vscode.instructions.md`
- `design-standards.design.instructions.md`
- `communication-style.vscode.instructions.md`
- `xml-tags.prompt-engineering.instructions.md`

**Agent Files Only:**
- `agent.design.instructions.md`

**Instructions Files Only:**
- `instructions.design.instructions.md`

**Prompt Files Only:**
- `prompt.design.instructions.md`

### Step 3: Extract Standards

For each applicable instruction file, identify:
- Frontmatter requirements
- Structure guidelines
- Naming conventions
- Compliance checklist items
- Forbidden patterns

### Step 4: Verify Compliance

Check the customization file against each standard:

**File Naming Convention:**
- [ ] Pattern: `{name}.{domain}.{type}.md`
- [ ] {name} uses kebab-case
- [ ] {domain} uses kebab-case
- [ ] {type} matches file type (agent, instructions, prompt)

**Design Standards:**
- [ ] YAML frontmatter present
- [ ] All required frontmatter fields present
- [ ] File structure follows prescribed format
- [ ] No cross-file markdown links
- [ ] Main heading matches file purpose

**Communication Style:**
- [ ] Imperative, direct language (no excessive politeness)
- [ ] Clear action-oriented tone
- [ ] No vague hedging language
- [ ] Consistent voice throughout

**XML Tags Best Practices:**
- [ ] Tag names lowercase with hyphens
- [ ] Tag names semantic and descriptive
- [ ] Tag names consistent throughout
- [ ] No empty tags
- [ ] Nesting logical (≤3 levels)
- [ ] Tag references in instructions use tag names
- [ ] Plural tags contain singular children

**Type-Specific Standards:**
- *For agents:* Apply `agent.design.instructions.md` checklist
- *For instructions:* Apply `instructions.design.instructions.md` checklist
- *For prompts:* Apply `prompt.design.instructions.md` checklist

### Step 5: Document Issues

For each non-compliant item, record:
- Standard violated
- Specific requirement
- Current state
- Recommended fix
- File location (line number if applicable)

### Step 6: Provide Recommendations

Organize recommendations by priority:

**Critical** (blocks compliance):
- File naming pattern incorrect
- Missing required frontmatter fields
- Invalid structure

**High** (breaks standards):
- Forbidden patterns present
- Inconsistent naming
- Formatting violations

**Medium** (best practice gaps):
- Empty tags
- Over-nested content
- Vague language

**Low** (polish items):
- Minor wording improvements
- Optional field additions

</instructions>

---

## Guidelines

- Focus on standards completeness, not code quality
- Reference instruction file sections by name when applicable
- Provide exact line numbers for location references
- Check frontmatter fields first before body content
- Prioritize file naming and structure over minor wording
- Flag related issues together (e.g., all XML tag issues in one section)

---

## Context

All applicable instruction files are located in `.github/instructions/*.instructions.md`. Use their `applyTo` patterns to determine relevance.