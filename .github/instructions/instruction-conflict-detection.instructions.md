---
name: Instruction Conflict Detection & Audit Protocol
description: Protocol for detecting and resolving conflicts between instruction files
applyTo: "**/*.instructions.md"
---

# Instruction Conflict Detection & Audit Protocol

This file defines how to detect, analyze, and resolve conflicts between instruction files. Use this protocol when creating new instructions or modifying existing ones.

---

## What is a Conflict?

A conflict exists when two instruction files define contradictory or overlapping rules:

| Conflict Type | Definition | Example |
|:---|:---|:---|
| **Contradiction** | Two files state opposite rules | safety-standards: "Never delete files" vs error-recovery: "Abort by cleaning up state" |
| **Duplication** | Same pattern in 2+ files | DRY principle in system-integrity AND in instruction-governance |
| **Precedence** | Unclear which rule wins | safety > performance? performance > speed? |
| **Scope Overlap** | `applyTo` patterns overlap | Both files apply to *.agent.md with different rules |
| **Dependency Loop** | Files depend on each other | File A references File B, File B references File A |

---

## Conflict Detection Matrix

Before publishing a new instruction file, check it against existing files:

### Safety-Standards vs Others

| File | Potential Conflict | Resolution |
|:---|:---|:---|
| error-recovery | "Never delete" vs "abort by cleanup" | Safety ALWAYS wins. Abort must not violate safety constraints. |
| tool-integration | Safety constraints vs tool usage | Tool usage must respect safety constraints. |
| prompt-engineering | Clear rules vs vague optimization | Clear safety rules trump optimization suggestions. |

### Error-Recovery vs Others

| File | Potential Conflict | Resolution |
|:---|:---|:---|
| tool-integration | Retry logic vs tool selection | Retry is fallback AFTER trying correct tool first. |
| reasoning-framework | CoT vs error recovery | Apply CoT (Pondering) BEFORE error recovery (when problem-solving). |
| system-integrity | Performance budgets vs error recovery | Error recovery > performance (safety first). |

### Tool-Integration vs Others

| File | Potential Conflict | Resolution |
|:---|:---|:---|
| system-integrity | Parallel execution vs token limits | Batch only independent calls; monitor token budget. |
| reasoning-framework | Tool selection vs thinking first | Think first (Observation/Pondering), THEN select tool. |

### System-Integrity vs Others

| File | Potential Conflict | Resolution |
|:---|:---|:---|
| agent-design | DRY rules vs agent specialization | DRY rules govern instruction layer (not agent logic). |
| prompt-engineering | Performance budgets vs clarity | Clarity first, then optimize within budget. |

---

## Conflict Detection Process

### Step 1: Pre-Publication Audit

Before publishing a new `.instructions.md` file:

```
FOR EACH section in new_file:
  FOR EACH existing_file in .github/instructions/:
    
    1. Semantic Match: Does this section cover similar topic?
       → If YES: Check for contradiction
       
    2. Exact Phrase Match: Do they define the same rule?
       → If YES: Potential duplication
       
    3. Scope Overlap: Do their `applyTo` patterns overlap?
       → If YES: Check for precedence clarity
       
    4. Dependency Check: Does this file reference the other?
       → If YES: Does the other reference back?
       → If YES to both: Circular dependency detected
```

### Step 2: Analyze Conflicts Found

For each potential conflict:

```
Conflict Type = ?

IF Contradiction:
  → Which rule should win? (decision needed)
  → Document precedence rule
  → Add to conflict resolution matrix
  
IF Duplication:
  → Consolidate to single file?
  → Or keep separate with explicit non-overlap?
  → Decision: Consolidate or explicitly scope
  
IF Precedence Unclear:
  → Add precedence rule to both files
  → Example: "Safety-Standards ALWAYS takes precedence"
  
IF Scope Overlap:
  → Clarify which file applies in each scenario
  → Example: "Safety-Standards applies to all, Tool-Integration for tool-specific"
  
IF Circular Dependency:
  → Break cycle: A → B (not B → A)
  → Or create new mediating file C
```

### Step 3: Resolution

Document resolution in the new file:

```markdown
## Known Conflicts & Resolutions

### Conflict: X vs Y

**Issue:** [Description]

**Resolution:** [How we resolved it]

**Precedence Rule:** [If applicable]

**Example:** [Concrete case showing how it works]
```

---

## Automated Detection Checklist

Run these checks programmatically:

### 1. DRY Score Check

```
Count duplicated lines across files:
  - Same constraint appears in 2+ files? Flag as duplication
  - Same pattern appears verbatim? Consolidate
  - Target: <5% duplication
```

### 2. Semantic Overlap Check

```
Extract key concepts from each file:
  - reasoning-framework: O.P.E.R.A., patterns, thinking
  - safety-standards: constraints, do-nots, boundaries
  - tool-integration: tool choice, execution, errors
  
If overlap >30%:
  → Investigate potential duplication
  → Decide consolidate vs coexist
```

### 3. Contradiction Check

```
Extract rules in form "DO X" or "DON'T X":
  safety-standards: "DON'T delete files"
  error-recovery: Includes "abort by cleanup"
  
If contradiction:
  → Flag and resolve
  → Document precedence
```

### 4. Scope Coverage

```
Analyze `applyTo` patterns:
  - Does each pattern serve unique purpose?
  - Do patterns overlap? If yes, do rules conflict?
  - Is coverage complete (no gaps)?
```

### 5. Dependency Graph

```
Build directed graph of file references:
  A → B (A references B)
  B → C
  C → A (circular!)
  
Target: Acyclic (no circular dependencies)
```

---

## Resolution Templates

### Template 1: Contradiction Resolution

```markdown
## Conflict: [File A] vs [File B]

**Apparent Contradiction:**
[File A says]: "{{rule_a}}"
[File B says]: "{{rule_b}}"

**Analysis:**
These rules actually [complement/contradict]:
[Explanation]

**Resolution:**
[File A] applies in [scenario_a]
[File B] applies in [scenario_b]

**Precedence Rule:**
IF both apply: [Which wins?]

**Example:**
Scenario: {{example_scenario}}
Rule A would say: {{interpretation_a}}
Rule B would say: {{interpretation_b}}
Applied rule: {{applied_rule}}
Result: {{outcome}}
```

### Template 2: Duplication Consolidation

```markdown
## Duplication Found: [Pattern Name]

**Locations:**
- File A: [section]
- File B: [section]

**Consolidation Decision:**
Move to [Primary File] (more authoritative)

**Action:**
1. Keep pattern in [Primary File]
2. In [Secondary File]: Add note "See: [Primary File] for definition"
3. Remove duplicate from [Secondary File]
4. Document in CHANGELOG

**Impact:**
- Affects agents: [list if any agent-specific changes needed]
- Requires migration: [if breaking change]
```

### Template 3: Scope Clarification

```markdown
## Scope Overlap: [File A] and [File B]

**Overlap:**
Both apply to: {{shared_scope}}

**Clarification:**
[File A] handles: {{scope_a}}
[File B] handles: {{scope_b}}

**Non-Overlapping Rule:**
[When to use A vs B]

**Precedence:**
IF both apply: [Which wins?]
```

---

## Monthly Audit Process

Every month, run conflict detection:

### Week 1: Scan

```
1. Grep all .instructions.md files for duplicated patterns
2. Build DRY score (target: <5% duplication)
3. Audit `applyTo` patterns for overlap
4. Check dependency graph for cycles
```

### Week 2: Analyze

```
1. Review conflicts found
2. Categorize: Contradiction / Duplication / Scope / Dependency
3. Assess impact (critical / medium / low)
4. Propose resolutions
```

### Week 3: Resolve

```
1. Consolidate duplications
2. Document precedence rules
3. Update files with resolutions
4. Test with sample agents
```

### Week 4: Report

```
1. Document changes in AUDIT_LOG
2. Update capabilities.md with findings
3. Plan next month's priorities
4. Archive monthly report
```

---

## Escalation Path

If conflicts cannot be resolved:

```
Level 1 (Try to resolve):
  → Analyze conflict definition
  → Propose resolution
  → If resolution found: Document and continue

Level 2 (Team discussion):
  → If conflict ambiguous: Route to ecosystem-orchestrator
  → Present both sides of conflict
  → Get explicit decision

Level 3 (Design decision):
  → If systemic conflict: Route to master-planner
  → May require refactoring instruction architecture
  → Document as precedent for future conflicts

Level 4 (Halt & escalate):
  → If unresolvable: Block publication of new file
  → Escalate to human maintainer
  → Cannot proceed without explicit approval
```

---

## Example: Detecting Real Conflict

### Scenario: New Instruction File Created

**File:** performance-optimization.instructions.md

**Excerpt:**
```
Use parallel execution for all independent tool calls.
This significantly improves speed.
```

### Audit Process

**Step 1: Check against safety-standards.instructions.md**
```
safety-standards says: "Validate all operations before executing"
performance-optimization says: "Batch operations in parallel"

Conflict? Possible - validation takes time, parallel skips sequential validation.
```

**Step 2: Check against error-recovery.instructions.md**
```
error-recovery says: "Retry failed operations with smaller scope"
performance-optimization says: "Batch all operations"

Conflict? Possible - batching might hide which operation failed.
```

**Step 3: Analyze**
```
Actual contradiction?
  - NO: These can coexist
  - Parallel execution ≠ skip validation
  - Batching ≠ prevent error detection
  - Retry ≠ incompatible with parallel
  
Scope issue?
  - YES: When to apply parallel vs sequential?
  - Need clarity: Parallel for independent reads, not for error recovery
```

**Step 4: Resolution**
```
Add to performance-optimization.instructions.md:

"Parallel Execution Safety:
  - Batch ONLY independent calls (no data dependencies)
  - Monitor for errors in parallel batch
  - If ANY call fails: Escalate entire batch to error-recovery
  - Example: 5 parallel reads OK. Sequential validation of results OK."
```

**Step 5: Document**
```
Add to conflict resolution section:

"Potential Conflict: Performance vs Error Recovery
  Resolution: Parallel execution for independent ops, then sequential validation.
  Example: [code example showing both]"
```

---

## Reference

- **Related:** instruction-governance.instructions.md (standards for writing instructions)
- **Related:** safety-standards.instructions.md (precedence rules)
- **Related:** system-integrity.instructions.md (DRY enforcement)
