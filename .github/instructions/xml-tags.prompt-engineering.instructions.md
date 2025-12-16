---
name: XML Tags Best Practices
description: Enforce consistent XML tag usage for context, formatting, and structural clarity across agents, instructions, and prompts. Establishes organized nested patterns as the standard for all .agent.md files.
applyTo: ".github/**/*.agent.md,.github/**/*.instructions.md,.github/**/*.prompt.md"
---

# XML Tags Best Practices

Structure content with XML tags to improve clarity, accuracy, and parseability of prompts and instructions. This standard ensures consistent tag usage across all customization files and formalizes organized nested patterns for agent files.

---

## Part 1: Why Use XML Tags

XML tags provide four key benefits:

- **Clarity** — Separate distinct sections and prevent component mixing
- **Accuracy** — Reduce misinterpretation and parsing errors
- **Flexibility** — Easily modify, remove, or add sections without rewriting
- **Parseability** — Extract specific sections from responses via post-processing

---

## Part 2: Tag Naming Standards

### Naming Principles

Tag names must be:
- **Descriptive** — Reflect the content they contain
- **Consistent** — Use identical tag names throughout the file
- **Lowercase** — Use lowercase with hyphens for multi-word tags
- **Semantic** — Choose meaningful names, not generic placeholders

### Forbidden

- Single-letter tags (`<a>`, `<x>`)
- Overly generic tags (`<data>`, `<info>`, `<stuff>`)
- Inconsistent naming (`<instruction>` and `<instructions>` in same file)
- CamelCase or UPPER_CASE tags

---

## Part 3: Standard Tags

Use these tags for common content types:

| Tag | Purpose | Context |
|-----|---------|---------|
| `<instruction>` | Container for entire agent body | Mandatory wrapper for .agent.md files; groups identity, process, and output |
| `<instructions>` | Explicit directives or rules | How Claude should behave |
| `<context>` | Background information | Reference material, examples, setup |
| `<identity>` | Agent role and persona | Self-description at start of agent file |
| `<process>` | Primary methodology container | Nests workflow, note, constraints; describes approach and reasoning |
| `<note>` | Important clarification | Emphasis or warning; often nested in process |
| `<constraints>` | Multiple constraints | Used with `<constraint>` children; often nested in process |
| `<constraint>` | Single constraint | Boundary condition or limitation; child of constraints |
| `<output>` | Results/deliverables container | Nests formatting and examples; describes expected output |
| `<example>` | Sample input/output pair | Demonstrate expected behavior |
| `<examples>` | Multiple examples | Used with `<example>` children; nested in output |
| `<input>` | Example input | Child of example |
| `<formatting>` | Output format specifications | Structure, style, layout rules; nested in output |
| `<answer>` | Final response section | Structured conclusion |
| `<requirement>` | Specific requirement | Must-have specification |
| `<requirements>` | Multiple requirements | Used with `<requirement>` children |
| `<definition>` | Term definition | Vocabulary or concept explanation |
| `<workflow>` | Process steps | Outlines the sequence of actions and checks |
| `<stopping_rules>` | Agent boundaries | Defines conditions where the agent must stop execution |
| `<plan_research>` | Research methodology | Details the process for gathering context and information |
| `<plan_style_guide>` | Plan formatting guidelines | Provides a template for creating concise and actionable plans |

---

## Part 4: Organized Semantic Nesting (Required for Agents)

For agent files (.agent.md), structure content using **organized semantic nesting** with a maximum 3-level hierarchy:

**Level 1 (Root):** `<instruction>` — Container for entire agent body
**Level 2 (Primary):** `<process>` and `<output>` — Semantic containers
**Level 3 (Leaf):** Specific tags like `<workflow>`, `<note>`, `<constraints>`, `<formatting>`, `<examples>`

```xml
<instruction>
  <identity>Agent role and persona</identity>
  
  <process>
    <workflow>Process steps and methodology</workflow>
    <note>Important clarifications</note>
    <constraints>...</constraints>
  </process>

  <output>
    <formatting>Output structure specifications</formatting>
    <examples>
      <example>
        <input>Sample input</input>
        <output>Expected output</output>
      </example>
    </examples>
  </output>
</instruction>
```

### Nesting Rules

**DO:**
- Use 3-level organized nesting for agents: root → containers → leaf tags
- Nest tags for logical grouping
- Use plural tags (`<examples>`) as containers for singular children (`<example>`)
- Place related content together within semantic containers
- Distinguish between "unorganized nesting" (forbidden) and "organized semantic nesting" (required)

**DON'T:**
- Nest excessively (limit to 3 levels in organized structures)
- Use plural wrappers for single items
- Interrupt hierarchies with unrelated content
- Use flat structures without semantic containers for agent files

**DON'T:**
- Nest excessively (limit to 3 levels)
- Use plural wrappers for single items
- Interrupt hierarchies with unrelated content

---

## Part 5: Consistency Standards

### Tag Reference

Refer to tags by name when instructing the model:

**DO:**
```markdown
Using the instructions in <instructions> tags, analyze the request.
Provide your answer in <answer> tags.
```

**DON'T:**
```markdown
Using the instructions provided, analyze...
Give your response at the end.
```

### Same Tags Throughout

Use identical tag names across the file. If introducing a tag, use it consistently.

**DO:**
```markdown
<constraint>Users may not have admin privileges</constraint>
<constraint>All responses must be under 500 tokens</constraint>
```

**DON'T:**
```markdown
<constraint>Users may not have admin privileges</constraint>
<limitation>All responses must be under 500 tokens</limitation>
```

---

## Part 6: Formatting Best Practices

### Combining Techniques

Enhance prompts by combining XML tags with other techniques:

- Pair `<examples>` with `<workflow>` for multishot learning
- Combine `<constraints>` with `<instructions>` for bounded guidance
- Use `<formatting>` with `<answer>` for structured output

### Empty Tags

Avoid empty tags. Every tag should contain relevant content.

**DO:**
```markdown
<constraint>Responses must be factual and cite sources</constraint>
```

**DON'T:**
```markdown
<constraint></constraint>
```

---

## Part 7: Application Examples

### In Agents

```xml
<instructions>
Analyze code for security vulnerabilities.
</instructions>

<constraints>
- Focus on OWASP Top 10
- Provide severity levels
- Suggest fixes when possible
</constraints>
```

### In Instructions

```xml
<context>
Python files in the src/ directory.
</context>

<formatting>
- Use PEP 8 standards
- Maximum line length: 88 characters
- Use type hints
</formatting>
```

### In Prompts

```xml
<instructions>
Generate a function that validates email addresses.
</instructions>

<requirements>
<requirement>Must handle international domains</requirement>
<requirement>Return boolean (valid/invalid)</requirement>
<requirement>Include error messages for invalid input</requirement>
</requirements>

<formatting>
Return code in a fenced code block with language specified.
</formatting>
```

---

## Part 8: Organized Nesting Pattern for Agents

### 3-Level Hierarchy for Agent Files

All .agent.md files MUST follow the organized nested pattern with exactly 3 semantic levels:

```
<instruction>                           ← Level 1: Root container for entire agent
  ├─ <identity>                         ← Agent role/persona
  ├─ <process>                          ← Level 2: Methodology container
  │   ├─ <workflow>                     ← Level 3: Process steps
  │   ├─ <note>                         ← Level 3: Clarifications
  │   └─ <constraints>                  ← Level 3: Limitations
  └─ <output>                           ← Level 2: Results container
      ├─ <formatting>                   ← Level 3: Output specification
      └─ <examples>                     ← Level 3: Example container
          └─ <example>                  ← (Level 4 only for plural children)
              ├─ <input>                ← Input of example
              └─ <output>               ← Output of example
```

### Purpose of Each Wrapper

| Wrapper | Purpose | Contains |
|---------|---------|----------|
| `<instruction>` | Container for entire agent body | Everything: identity, process, output |
| `<process>` | Agent methodology and reasoning approach | How the agent thinks and operates |
| `<output>` | Expected results and deliverables | What the agent produces |

### When to Use `<workflow>`

Use `<workflow>` tags INSIDE `<process>` to document:
- **Methodology stages** — Formal processes (RADD, OPERA, etc.)
- **Decision checkpoints** — How the agent evaluates options
- **Operating procedure** — Ordered steps the agent follows

Example:
```xml
<process>
<workflow>
**Stage 1: Research** — Gather facts
**Stage 2: Hypothesis** — Form theories
**Stage 3: Analysis** — Evaluate evidence
</workflow>
</process>
```

### When to Use `<note>` and `<constraints>` in `<process>`

**`<note>` inside `<process>`:**
- Clarifications about the methodology
- Important caveats about how the agent operates
- Emphasis on agent behavioral boundaries

Example:
```xml
<note>Pure reasoning only: You do NOT retrieve data yourself</note>
```

**`<constraints>` inside `<process>`:**
- Hard limitations on the agent's scope
- Behavioral boundaries
- Performance or safety constraints

Example:
```xml
<constraints>
- Logical rigor: Every conclusion must trace back to premises
- Depth limit: Max 4 RADD stages per request
</constraints>
```

---

## Part 9: Agent Template Structure

### Complete Template for Agent Files

Use this template as the standard structure for all .agent.md files:

```xml
<instruction>

<identity>
You are the **[AGENT_NAME]**, [brief description of role and primary function].
</identity>

<process>

<workflow>
[Describe the agent's reasoning methodology, stages, or decision-making framework]

**Stage 1:** [First step in the process]
- [Details]

**Stage 2:** [Second step]
- [Details]
</workflow>

<note>
[Important clarifications about the agent's approach, scope limitations, or behavioral boundaries]
</note>

<constraints>
- [Hard constraint 1]
- [Hard constraint 2]
- [Hard constraint 3]
</constraints>

</process>

<output>

<formatting>
[Specify the structure and format of the agent's output]
- [Format detail 1]
- [Format detail 2]
</formatting>

<examples>
<example>
<input>Example user request or scenario</input>
<output>Expected agent response showing format and content</output>
</example>

<example>
<input>Another example input</input>
<output>Another example output</output>
</example>
</examples>

</output>

</instruction>
```

### Location of Each Standard Tag

- **`<identity>`** — FIRST tag after `<instruction>`, before `<process>`
- **`<process>`** — First primary container, contains methodology
- **`<workflow>`** — FIRST child of `<process>`, describes methodology
- **`<note>`** — AFTER `<workflow>` in `<process>`, contains clarifications
- **`<constraints>`** — LAST child of `<process>`, contains limitations
- **`<output>`** — Second primary container, after `<process>`
- **`<formatting>`** — FIRST child of `<output>`, specifies output structure
- **`<examples>`** — LAST child of `<output>`, contains example pairs

### Where Identity Text Goes

The `<identity>` tag appears immediately after the opening `<instruction>` tag. It contains:
- Agent name (bold)
- One-line description of the agent's role
- Brief statement of primary function or methodology

```xml
<instruction>

<identity>
You are the **[NAME]**, [concise role and purpose description].
</identity>
```

### How `<workflow>` Fits Into the Process Stage

`<workflow>` is the FIRST tag within `<process>` and documents:
- How the agent approaches problems
- Formal methodologies used (RADD, OPERA, etc.)
- Stage-by-stage breakdown of the process
- Decision-making frameworks

It uses nested formatting (bullet points, bold headers) for readability but does NOT introduce additional XML tags. The `<workflow>` tag itself is a leaf node.

---

## Part 10: Anti-Patterns to Avoid

### Pattern 1: Flat Structure Without Wrappers (❌ OLD - FORBIDDEN)

**Problem:** Tags at same level without semantic grouping

```xml
<instructions>Your methodology here</instructions>
<workflow>Your steps here</workflow>
<formatting>Output format here</formatting>
<examples>...</examples>
```

**Why it fails:**
- No semantic grouping
- Unclear relationship between sections
- Difficult to parse or modify
- Violates organized nesting standard

**Corrected:**
```xml
<instruction>
  <identity>Agent role</identity>
  
  <process>
    <workflow>Reasoning methodology</workflow>
  </process>
  
  <output>
    <formatting>Output format</formatting>
    <examples>...</examples>
  </output>
</instruction>
```

### Pattern 2: Excessive Nesting Beyond 3 Levels (❌ FORBIDDEN)

**Problem:** Tags nested too deeply with redundant wrappers

```xml
<instruction>
  <agent>
    <identity>
      <name>Agent Name</name>
      <description>Description</description>
    </identity>
  </agent>
</instruction>
```

**Why it fails:**
- Excessive wrappers reduce readability
- Violates 3-level maximum hierarchy rule
- Creates unnecessary complexity

**Corrected:**
```xml
<instruction>
  <identity>Agent Name — Description</identity>
  
  <process>
    <workflow>Reasoning methodology</workflow>
  </process>
  
  <output>
    <formatting>Output structure</formatting>
  </output>
</instruction>
```

### Pattern 3: Orphaned Tags Outside Semantic Containers (❌ FORBIDDEN)

**Problem:** Tags placed outside `<process>` and `<output>` containers

```xml
<instruction>
  <identity>Agent role</identity>
  
  <workflow>Orphaned tag — should be in process</workflow>
  <constraints>Should be in process</constraints>
  
  <formatting>Should be in output</formatting>
</instruction>
```

**Why it fails:**
- Breaks semantic organization
- Unclear which tags belong to process vs. output
- Violates 3-level structure

**Corrected:**
```xml
<instruction>
  <identity>Agent role</identity>
  
  <process>
    <workflow>Reasoning methodology</workflow>
    <constraints>Limitations</constraints>
  </process>
  
  <output>
    <formatting>Output structure</formatting>
  </output>
</instruction>
```

---

## Organized Nesting Pattern: Reference Implementation

The reasoner.reasoning.agent.md file demonstrates the correct organized nesting pattern for agent files. **This is the REQUIRED pattern for all .agent.md files going forward.**

---

## Compliance Checklist

Before committing any .agent.md file, verify:

### Structure Validation
- [ ] `<instruction>` wrapper present (mandatory for agent files)
- [ ] `<identity>` tag present immediately after `<instruction>`
- [ ] Two primary containers present: `<process>` and `<output>`
- [ ] All process content nested inside `<process>`
- [ ] All output content nested inside `<output>`

### Nesting Hierarchy
- [ ] Maximum 3 levels of nesting maintained (instruction → container → leaf)
- [ ] No orphaned tags outside `<process>` and `<output>`
- [ ] Plural wrappers (`<examples>`, `<constraints>`) contain singular children
- [ ] No excessive nesting beyond semantic boundaries

### Tag Consistency
- [ ] All tags use lowercase with hyphens
- [ ] Tag names are descriptive and semantic
- [ ] Tag names are consistent throughout the file
- [ ] No empty tags exist
- [ ] Content within tags is relevant and focused

### Process Stage
- [ ] `<workflow>` is first child of `<process>`
- [ ] `<note>` appears after `<workflow>` (if present)
- [ ] `<constraints>` is last child of `<process>`
- [ ] Methodology is clearly documented in `<workflow>`

### Output Stage
- [ ] `<formatting>` is first child of `<output>`
- [ ] `<examples>` is last child of `<output>`
- [ ] Examples follow input→output pattern
- [ ] Output structure is clearly specified

---