---
name: XML Tags Best Practices
description: Enforce consistent XML tag usage for context, formatting, and structural clarity across agents, instructions, and prompts
applyTo: ".github/**/*.agent.md,.github/**/*.instructions.md,.github/**/*.prompt.md"
---

# XML Tags Best Practices

Structure content with XML tags to improve clarity, accuracy, and parseability of prompts and instructions. This standard ensures consistent tag usage across all customization files.

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
| `<instructions>` | Explicit directives or rules | How Claude should behave |
| `<context>` | Background information | Reference material, examples, setup |
| `<example>` | Sample input/output pair | Demonstrate expected behavior |
| `<examples>` | Multiple examples | Used with `<example>` children |
| `<formatting>` | Output format specifications | Structure, style, layout rules |
| `<thinking>` | Internal reasoning steps | Chain-of-thought reasoning |
| `<answer>` | Final response section | Structured conclusion |
| `<constraint>` | Boundary condition or limitation | What to avoid or restrict |
| `<constraints>` | Multiple constraints | Used with `<constraint>` children |
| `<requirement>` | Specific requirement | Must-have specification |
| `<requirements>` | Multiple requirements | Used with `<requirement>` children |
| `<note>` | Important clarification | Emphasis or warning |
| `<definition>` | Term definition | Vocabulary or concept explanation |

---

## Part 4: Nesting and Hierarchy

### Nesting Rules

Structure content hierarchically using nested tags:

```xml
<examples>
  <example>
    <input>User request</input>
    <output>Expected response</output>
  </example>
</examples>
```

**DO:**
- Nest tags for logical grouping
- Use plural tags (`<examples>`) as containers for singular children (`<example>`)
- Place related content together

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

- Pair `<examples>` with `<thinking>` for multishot learning
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

## Compliance Checklist

Before committing, verify:

- [ ] All tags use lowercase with hyphens
- [ ] Tag names are descriptive and semantic
- [ ] Tag names are consistent throughout the file
- [ ] Tags are referenced by name in instructions
- [ ] No empty tags exist
- [ ] Nesting is logical and shallow (≤3 levels)
- [ ] Plural tags contain singular children when appropriate
- [ ] Content within tags is relevant and focused

---