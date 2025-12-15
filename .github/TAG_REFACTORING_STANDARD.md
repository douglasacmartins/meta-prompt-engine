# Unified Tag Refactoring Standard

**Version:** 1.0  
**Date:** December 15, 2025  
**Scope:** All agent definitions across all domains (.core, .quality, .creation, .workflow, .infrastructure, .reasoning)  
**Purpose:** Establish consistent tag content formatting to eliminate markdown formatting from inside XML tags

---

## Executive Summary

Agent definition files currently mix markdown formatting with XML tag content, creating inconsistency and readability issues. This standard defines a unified approach where:

- **Tags contain ONLY:**
  - Plain text (no markdown syntax)
  - Plain text lists (leading dashes)
  - Tables (pipe-delimited)
  - Code blocks (triple backticks)

- **Tags do NOT contain:**
  - Markdown headers (`#`, `##`, `###`, etc.)
  - Bold formatting (`**text**`)
  - Italic formatting (`*text*` or `_text_`)
  - Inline code (`` `text` ``)
  - Strikethrough (`~~text~~`)
  - Links (`[text](url)`)
  - Other markdown syntax

---

## Tag Content Rules

### ✅ ALLOWED Inside Tags

| Element | Format | Example |
|---------|--------|---------|
| Plain text | Direct text, no formatting | `Receive the incoming request` |
| Numbered lists | Standard numbering | `1. First step` |
| Bullet lists | Leading dashes/asterisks | `- Item one` |
| Tables | Pipe-delimited markdown tables | See section below |
| Code blocks | Triple-backtick fenced blocks | See section below |
| Line breaks | Blank lines for separation | Yes, allowed |

### ❌ NOT ALLOWED Inside Tags

| Element | Incorrect | Correct |
|---------|-----------|---------|
| Headers | `# Header Text` | `Header Text` |
| Bold | `**Bold Text**` | `Bold Text` |
| Italic | `*Italic Text*` | `Italic Text` |
| Inline code | `` `variable` `` | `variable` |
| Links | `[Link](url)` | `Link` |
| Strikethrough | `~~Deleted~~` | `Deleted` |
| Emphasis | `***Bold Italic***` | `Text` |

---

## Structure Pattern: Plain Text Organization

### Pattern 1: Section with Introduction + Bullet Points

**BEFORE (Incorrect):**
```xml
<instructions>
# Step 1: Extract Requirements

Extract and parse the user request:

- Parse user request for scope
- Identify constraints
- Map to agent capabilities

# Step 2: Route Analysis

Delegate to specialists:

- Choose specialist agents
- Send handoff requests
</instructions>
```

**AFTER (Correct):**
```xml
<instructions>
Step 1: Extract Requirements
Extract and parse the user request:
- Parse user request for scope
- Identify constraints
- Map to agent capabilities

Step 2: Route Analysis
Delegate to specialists:
- Choose specialist agents
- Send handoff requests
</instructions>
```

**Changes Made:**
- Removed `# ` header markers
- Kept introductory plain text
- Maintained bullet lists unchanged
- Used line spacing to separate sections

---

### Pattern 2: Numbered Steps with Details

**BEFORE (Incorrect):**
```xml
<process>
# Your Process

1. **Receive Plan** — Accept structured plan from master-planner or user
2. **Classify Execution Type** — Determine scope
3. **Route to Specialists** — Delegate via handoffs
</process>
```

**AFTER (Correct):**
```xml
<process>
Your Process

1. Receive Plan — Accept structured plan from master-planner or user
2. Classify Execution Type — Determine scope
3. Route to Specialists — Delegate via handoffs
</process>
```

**Changes Made:**
- Removed `# ` header
- Removed `**bold**` formatting around item titles
- Kept numbered list format unchanged
- Kept em-dashes as separators (plain text)

---

### Pattern 3: Key-Value Pairs (Type Definitions)

**BEFORE (Incorrect):**
```xml
<execution_types>
**Type 1: Implementation** — Build new artifacts
- Route: @meta-prompt-architect
- Output: New files ready for deployment

**Type 2: Optimization** — Refine existing templates
- Route: @meta-prompt-optimizer
- Output: Enhanced templates
</execution_types>
```

**AFTER (Correct):**
```xml
<execution_types>
Type 1: Implementation — Build new artifacts
- Route: @meta-prompt-architect
- Output: New files ready for deployment

Type 2: Optimization — Refine existing templates
- Route: @meta-prompt-optimizer
- Output: Enhanced templates
</execution_types>
```

**Changes Made:**
- Removed `**bold**` formatting from type names
- Kept em-dashes as separators
- Maintained nested bullet lists

---

### Pattern 4: Simple Text Content

**BEFORE (Incorrect):**
```xml
<note>
Never modify the plan; execute as specified. Route all execution to appropriate *specialists* via handoffs; do **NOT** implement directly.
</note>
```

**AFTER (Correct):**
```xml
<note>
Never modify the plan; execute as specified. Route all execution to appropriate specialists via handoffs; do NOT implement directly.
</note>
```

**Changes Made:**
- Removed `*emphasis*` formatting
- Removed `**bold**` formatting
- Kept all text content

---

### Pattern 5: Tables (PRESERVED)

Tables are ALLOWED and should be preserved exactly as-is:

```xml
<execution_types>
Type Definitions

| Type | Description | Specialist |
|------|-------------|-----------|
| Implementation | Build new artifacts | @meta-prompt-architect |
| Optimization | Refine templates | @meta-prompt-optimizer |
| Validation | Safety checks | @meta-prompt-critic |
</execution_types>
```

**No changes needed to tables.**

---

### Pattern 6: Code Blocks (PRESERVED)

Code blocks with triple backticks are ALLOWED and should be preserved:

```xml
<example>
Example Input

Generate a simple agent with this structure:

```yaml
name: sample-agent
description: Sample agent
tags: [demo]
```
</example>
```

**No changes needed to code blocks.**

---

## Complete Before/After Transformation Example

### BEFORE (Current State with Violations)

```xml
<instruction>

# Identity

You are the **Executor**.
Execute finalized plans by orchestrating implementation, optimization, and deployment across the agent ecosystem.

<process>

# Your Process

1. **Receive Plan** — Accept structured plan from master-planner or user
2. **Classify Execution Type** — Determine execution scope (implementation, optimization, deployment, lifecycle)
3. **Route to Specialists** — Delegate to appropriate agents via handoffs
4. **Coordinate Workflow** — Manage dependencies and sequencing between execution stages
5. **Monitor Progress** — Track completion status and escalate blockers
6. **Verify Outcomes** — Confirm execution results match plan specifications

</process>

<note>
Never modify the plan; execute as specified. Route all execution to appropriate *specialists* via handoffs; do **NOT** implement directly. This ensures quality and maintains separation of concerns.
</note>

## Execution Types

**Type 1: Implementation** — Build new artifacts (agents, prompts, instructions)
- Route: @meta-prompt-architect
- Output: New agent/prompt/instruction files ready for deployment

**Type 2: Optimization** — Refine existing templates and workflows
- Route: @meta-prompt-optimizer
- Output: Enhanced templates with performance improvements

<constraints>
- Never modify the plan; execute as specified.
- Route all execution to appropriate specialists; do NOT implement directly.
- Escalate blockers to @ecosystem-orchestrator if specialist unavailable.
- Verify each execution step completes before proceeding to next.
</constraints>
```

### AFTER (Refactored)

```xml
<instruction>

Identity

You are the Executor.
Execute finalized plans by orchestrating implementation, optimization, and deployment across the agent ecosystem.

<process>

Your Process

1. Receive Plan — Accept structured plan from master-planner or user
2. Classify Execution Type — Determine execution scope (implementation, optimization, deployment, lifecycle)
3. Route to Specialists — Delegate to appropriate agents via handoffs
4. Coordinate Workflow — Manage dependencies and sequencing between execution stages
5. Monitor Progress — Track completion status and escalate blockers
6. Verify Outcomes — Confirm execution results match plan specifications

</process>

<note>
Never modify the plan; execute as specified. Route all execution to appropriate specialists via handoffs; do NOT implement directly. This ensures quality and maintains separation of concerns.
</note>

Execution Types

Type 1: Implementation — Build new artifacts (agents, prompts, instructions)
- Route: @meta-prompt-architect
- Output: New agent/prompt/instruction files ready for deployment

Type 2: Optimization — Refine existing templates and workflows
- Route: @meta-prompt-optimizer
- Output: Enhanced templates with performance improvements

<constraints>
- Never modify the plan; execute as specified.
- Route all execution to appropriate specialists; do NOT implement directly.
- Escalate blockers to @ecosystem-orchestrator if specialist unavailable.
- Verify each execution step completes before proceeding to next.
</constraints>
```

### Changes Summary

- Removed 7 markdown headers (`#`, `##`)
- Removed 8 bold formatting instances (`**text**` → `text`)
- Removed 2 italic formatting instances (`*text*` → `text`)
- Removed 1 emphasis (`do **NOT**` → `do NOT`)
- Preserved all 2 bullet lists
- Preserved all 1 numbered lists
- Result: Clean, consistent plain text within tags

---

## Refactoring Checklist

Use this checklist for each agent file in the ecosystem:

### Pre-Refactor Verification
- [ ] File is a valid `.agent.md` file
- [ ] File contains XML-like tags (e.g., `<instruction>`, `<process>`, `<constraints>`)
- [ ] File contains markdown formatting inside tags (headers, bold, italic, etc.)

### Header Removal
- [ ] Search for all `#` characters at line start (line headers)
- [ ] Replace `# Header Text` with `Header Text`
- [ ] Replace `## Subheader Text` with `Subheader Text`
- [ ] Verify no `#` markdown headers remain

### Bold Removal
- [ ] Search for all `**text**` patterns within tags
- [ ] Replace `**text**` with `text`
- [ ] Verify no bold formatting remains

### Italic Removal
- [ ] Search for all `*text*` or `_text_` patterns within tags
- [ ] Replace `*text*` with `text`
- [ ] Replace `_text_` with `text`
- [ ] Verify no italic formatting remains

### Inline Code Removal
- [ ] Search for all `` `text` `` patterns within tags
- [ ] Replace `` `text` `` with `text`
- [ ] Exception: Keep code blocks (triple backticks) unchanged
- [ ] Verify no inline code remains

### Other Markdown Cleanup
- [ ] Search for and remove strikethrough (`~~text~~` → `text`)
- [ ] Search for and remove links (keep URL or text, remove markdown syntax)
- [ ] Search for and remove HTML tags within tags

### Content Preservation
- [ ] Verify all bullet lists (`-`, `*`) are preserved
- [ ] Verify all numbered lists (`1.`, `2.`, etc.) are preserved
- [ ] Verify all tables are preserved unchanged
- [ ] Verify all code blocks (triple backticks) are preserved

### Format Verification
- [ ] All section separations use line breaks, not headers
- [ ] All emphasis uses plain text, not markdown
- [ ] All lists maintain proper indentation
- [ ] File opens and closes all XML-like tags properly

### Final Quality Check
- [ ] File is valid and parseable
- [ ] No markdown syntax visible inside tags
- [ ] All content is plain text, lists, tables, or code blocks
- [ ] Git diff shows only formatting changes, no content loss

---

## Step-by-Step Refactoring Process

### Step 1: Backup and Branch
```
git checkout -b refactor/tag-standard
cp agent-file.md agent-file.md.backup
```

### Step 2: Open File in Editor
Open the agent file and review all tagged sections.

### Step 3: Find & Replace - Markdown Headers

For each header level in the file:

1. Open Find & Replace (`Ctrl+H`)
2. Enable Regular Expressions toggle
3. Find: `^# (.+)$` (headers level 1)
4. Replace: `$1`
5. Replace all matches
6. Repeat for `^## (.+)$`, `^### (.+)$`, etc.

### Step 4: Find & Replace - Bold Text

1. Open Find & Replace
2. Disable Regular Expressions
3. Find: `**`
4. Review each match:
   - If inside a tag: Replace with nothing (removes both `**` markers)
   - If outside tags: Consider context, may skip
5. After all `**` removed: Replace each remaining `**` with `**` (should be none)
   - Result: `**bold**` becomes `bold`

### Step 5: Find & Replace - Italic Text

1. Open Find & Replace
2. Enable Regular Expressions
3. Find: `\*([^*]+)\*` (text wrapped in single asterisks)
4. Replace: `$1`
5. Repeat for underscores: Find `_([^_]+)_`
6. Replace: `$1`

### Step 6: Find & Replace - Inline Code

1. Open Find & Replace
2. Disable Regular Expressions
3. Find: `` ` ``
4. Review each match:
   - Inside tags, inside triple backticks: Skip (part of code block)
   - Inside tags, standalone: Replace pair with nothing
   - Outside tags: Keep for now

### Step 7: Visual Review

1. Scroll through entire file
2. Look for remaining markdown syntax inside tags
3. Check for proper spacing between sections
4. Verify lists and tables are intact
5. Verify code blocks are intact

### Step 8: Git Diff Review

```bash
git diff agent-file.md
```

Verify:
- Only formatting changes visible
- No accidental content deletion
- All tags properly closed
- Structure preserved

### Step 9: Test & Validate

1. Copy file to working directory
2. Test file parsing (if applicable)
3. Verify no syntax errors
4. Confirm semantic meaning unchanged

### Step 10: Commit

```bash
git add agent-file.md
git commit -m "refactor: apply tag refactoring standard to [agent-name]"
```

---

## Format Guidance: Organizing Plain Text Without Markdown

### Technique 1: Section Headings with Line Breaks

Use plain text labels followed by line breaks. No `#` markers.

```
Section Title

Content goes here. This is the body of the section.
It can span multiple lines.

Another paragraph here with more details.
```

### Technique 2: Subsection Labels Without Hierarchy

When you need subsections, use plain text labels with indentation:

```
Main Section

  Subsection One
  Description of subsection one.
  
  Subsection Two
  Description of subsection two.
```

### Technique 3: Emphasis Through Structure, Not Formatting

Instead of bold (`**word**`), use:
- Placement at the start of a line
- Capital letters for acronyms/important terms
- Em-dashes for emphasis: `This is IMPORTANT — read carefully`
- Repetition or direct statements

**Instead of:** `This is **absolutely critical**`  
**Use:** `This is absolutely critical` OR `CRITICAL: This must not be modified`

### Technique 4: Numbered Steps (Preserved)

Numbered lists work perfectly without markdown:

```
Process Steps

1. First step description here
2. Second step description here
3. Third step description here
```

### Technique 5: Bullet Points (Preserved)

Bullet lists work perfectly without markdown:

```
Characteristics

- Item one with description
- Item two with description
- Item three with description
```

### Technique 6: Key-Value Pairs with Dashes

For pairs of information, use em-dashes on one line:

```
Agent Types

Type A — Used for processing requests and routing decisions
Type B — Used for validation and quality assurance
Type C — Used for optimization and performance tuning
```

### Technique 7: Indented Description Lists

For term-description pairs:

```
Execution Modes

Implementation
  Build new artifacts and integrate into ecosystem
  Route to appropriate specialists
  
Optimization
  Refine existing templates for better performance
  Validate improvements against baselines
  
Validation
  Perform comprehensive safety and quality checks
  Generate detailed assessment reports
```

### Technique 8: Tables for Complex Data

Tables are fully allowed and recommended for structured data:

```
| Element | Purpose | Example |
|---------|---------|---------|
| Implementation | Build new artifacts | agents, prompts |
| Validation | Safety checks | security review |
| Optimization | Performance tuning | template refinement |
```

### Technique 9: Code Blocks Unchanged

Triple-backtick code blocks are completely unaffected:

```yaml
agent:
  name: example
  type: core
  capabilities: [process, route, validate]
```

### Technique 10: Spacing for Readability

Use blank lines to separate logical sections:

```
Section A

First concept description here.
Additional context and details.

Section B

Second concept description here.
More context and examples.

Section C

Third concept here.
```

---

## Summary of Formatting Transformations

| Current (Incorrect) | Target (Correct) | Pattern |
|-------------------|------------------|---------|
| `# Header` | `Header` | Remove markdown header |
| `**bold**` | `bold` | Remove bold markers |
| `*italic*` | `italic` | Remove italic markers |
| `` `code` `` | `code` | Remove inline code backticks |
| `[link](url)` | `link` or `url` | Remove link syntax |
| `~~struck~~` | `struck` | Remove strikethrough |
| `- list` | `- list` | Keep bullets unchanged |
| `1. list` | `1. list` | Keep numbers unchanged |
| `` ``` `` code `` ``` `` | `` ``` `` code `` ``` `` | Keep code blocks unchanged |
| ` \| table \| ` | ` \| table \| ` | Keep tables unchanged |

---

## Domain-Specific Application

This standard applies uniformly across all agent domains:

- **.core** agents (executor, master-planner, ecosystem-orchestrator)
- **.quality** agents (meta-prompt-critic, pattern-validator)
- **.creation** agents (meta-prompt-architect, meta-specialist-factory, meta-prompt-optimizer)
- **.workflow** agents (meta-prompter, handoff-optimizer)
- **.infrastructure** agents (ecosystem-auditor, conflict-detection, meta-lifecycle-manager)
- **.reasoning** agents (reasoner, computational-thinking, synthetic-analyst, opera)

All follow the same tag content rules regardless of domain.

---

## Validation Checklist for Refactored Files

After refactoring, run through this validation:

- [ ] No `#` headers found in tag content
- [ ] No `**text**` found in tag content
- [ ] No `*text*` found in tag content
- [ ] No `` `text` `` found in tag content (outside code blocks)
- [ ] All bullet lists preserved
- [ ] All numbered lists preserved
- [ ] All tables preserved
- [ ] All code blocks preserved
- [ ] File is syntactically valid
- [ ] Semantic content unchanged
- [ ] Proper spacing maintained
- [ ] All tags properly closed

---

## Common Mistakes to Avoid

| Mistake | Problem | Solution |
|---------|---------|----------|
| Removing content by mistake | Accidentally delete explanatory text | Review git diff carefully before commit |
| Breaking code blocks | Modifying triple backticks | Never edit inside code blocks |
| Breaking tables | Removing pipe characters | Never edit inside tables |
| Incomplete replacement | Missing some instances | Use find & replace, not manual |
| Wrong scope | Formatting outside tags | Only modify content inside XML-like tags |
| List corruption | Changing list indentation | Preserve indentation exactly |
| Semantic loss | Removing nuance through formatting | Ensure plain text explanation is clear |

---

## Questions & Clarifications

**Q: Can I use ALL CAPS for emphasis?**  
A: Yes. Plain text alternatives to bold include ALL CAPS, placement at section start, or em-dash notation.

**Q: What about links inside tags?**  
A: Remove markdown link syntax `[text](url)`. Keep the URL or display text as appropriate.

**Q: Can lists have sub-lists?**  
A: Yes, use indentation. Lists remain allowed in their entirety.

**Q: What about nested tables?**  
A: Markdown doesn't support nested tables. Create separate tables if needed.

**Q: Should I refactor test files or examples?**  
A: Focus on agent definition files (`.agent.md`). Example code within code blocks is not refactored.

**Q: How do I handle descriptions with markdown-like symbols?**  
A: Use context. The symbol `*` in "3 * 5 = 15" is math, not markdown. Leave it alone.

---

## Rollout Timeline

- **Phase 1:** Core agents (.core domain) — Week 1
- **Phase 2:** Quality agents (.quality domain) — Week 2
- **Phase 3:** Creation agents (.creation domain) — Week 3
- **Phase 4:** Workflow agents (.workflow domain) — Week 4
- **Phase 5:** Infrastructure agents (.infrastructure domain) — Week 5
- **Phase 6:** Reasoning agents (.reasoning domain) — Week 6
- **Phase 7:** Validation and cleanup — Week 7

---

## References

- Agent Definition Format: `.github/agents/`
- Related Standards: XML tag naming, indentation consistency
- Maintenance: Review and update this standard annually or after major ecosystem changes

---

**Document Status:** APPROVED FOR ECOSYSTEM-WIDE ADOPTION  
**Last Updated:** December 15, 2025  
**Maintained By:** Meta-Prompt Engine Governance Committee
