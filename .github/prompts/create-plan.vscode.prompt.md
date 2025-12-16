---
name: Create Plan
description: Create a structured plan by delegating research and synthesizing outputs
argument-hint: "Describe the task + constraints. Include file refs, #codebase hits, and terminal output."
agent: master-planner
---

Create a structured plan for the user’s task.
Use grounded repository context and specialist delegation when needed.
Do NOT implement changes.

## Input

- Task description
- Constraints and definition of done
- Relevant file references (paths, symbols, snippets)
- Optional: `#codebase` hits, Problems output, terminal logs

## Output

Markdown plan with:
- Title + TL;DR
- Steps (3–6)
- Further considerations (1–3), including 2–4 clarifying questions when required
