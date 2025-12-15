# Reasoning Agents Interaction Guide

**Overview:** Three specialized reasoning agents work together to provide comprehensive validation and analysis of complex problems.

---

## The Three Reasoning Agents

### 1. @opera — Multi-Hop Orchestration
**Methodology:** OPERA (Orchestrated Planner-Executor Reasoning Architecture)

**Specialization:** Decomposes complex multi-step problems into manageable sub-goals with explicit dependency tracking

**What it does:**
- Breaks queries into atomic sub-goals: `[entity from step N]`
- Tracks execution flow through Goal Planning Module (GPM), Reason-Execute Module (REM), Trajectory Memory Component (TMC)
- Manages information sufficiency assessment and adaptive rewriting

**Best for:**
- Multi-hop reasoning chains
- Queries requiring sequential dependencies
- Problems where the solution path matters, not just the answer
- Complex decomposition with explicit step ordering

**Example Use Case:**
```
"Who directed the movie that starred the actor who played the wizard in Harry Potter?"
→ OPERA breaks into atomic steps with dependencies
→ Produces plan + trajectory log showing each step's rationale
```

---

### 2. @reasoner — Formal Logic Validation
**Methodology:** RADD (Research-Abductive-Dialectic-Deductive)

**Specialization:** Validates logical soundness and identifies contradictions using formal logic

**What it does:**
- **Research:** Gathers facts and establishes premises
- **Abductive:** Infers hidden mechanisms and competing hypotheses
- **Dialectic:** Debates theories, resolves contradictions
- **Deductive:** Verifies conclusions against axioms and constraints

**Best for:**
- Validating plans for logical consistency
- Identifying contradictions and axiom violations
- Ensuring conclusions follow from premises
- Formal verification of reasoning chains

**Example Use Case:**
```
"Verify: Should we add search tools to reasoning agents?"
→ RADD Research: Current state = pure reasoning only
→ RADD Abductive: Hypotheses = efficiency vs separation of concerns
→ RADD Dialectic: Tensions between rapid access vs clean architecture
→ RADD Deductive: Axiom check = "Reasoning agents are pure cognition"
→ Conclusion: Reject tools; use handoff pattern instead
```

---

### 3. @synthetic-analyst — Systemic Analysis
**Methodology:** Hermeneutic Cycle (Analysis-Synthesis-Collision-Conclusion)

**Specialization:** Examines problems holistically, resolving tensions between local fixes and system-wide impact

**What it does:**
- **Deconstruction:** Breaks problem into atomic facts and mechanisms
- **Contextualization:** Maps components to larger system and dependencies
- **Collision:** Resolves tension between analytical fix and systemic purpose
- **Unified Conclusion:** Produces diagnostic + fix + impact assessment

**Best for:**
- Understanding system-wide implications
- Resolving conflicts between efficiency and safety
- Choosing between multiple solution strategies
- Assessing broader impact of local changes

**Example Use Case:**
```
"Why did adding tools to reasoning agents break semantics?"
→ Deconstruction: Tools = data retrieval concern; reasoning agents = cognition only
→ Contextualization: System impact = coupling increases, separation breaks
→ Collision: Efficiency vs architecture; resolution = use handoffs
→ Conclusion: Diagnostic (tool coupling), Fix (remove tools, add handoffs), Impact (restored clarity)
```

---

## Interaction Patterns

### Pattern 1: Single Agent
**When:** Straightforward problem in agent's specialty

```
User Query
    ↓
@opera (for decomposition)
    ↓
Answer
```

### Pattern 2: Planning → Validation
**When:** Need both decomposition and logical verification

```
User Query
    ↓
@opera (decompose into sub-goals)
    ↓
Plan + Trajectory Log
    ↓
@reasoner (validate logic)
    ↓
Validation Report
    ↓
Approved Plan
```

### Pattern 3: Full Validation Chain
**When:** Complex problem needs decomposition, logic check, and systemic review

```
User Query
    ↓
@opera (create decomposition)
    ↓
Plan + Trajectory Log
    ↓
@reasoner (verify logical chain)
    ↓
Validation Report
    ↓
@synthetic-analyst (check systemic impact)
    ↓
Systemic Impact Assessment
    ↓
Ready for Implementation
```

### Pattern 4: Systemic Analysis First
**When:** Problem appears to be system-wide, but needs decomposition to understand

```
User Query (systemic concern)
    ↓
@synthetic-analyst (analyze holistically)
    ↓
Root Cause Analysis
    ↓
Suggests Decomposition Needed
    ↓
@opera (break down into steps)
    ↓
Actionable Plan
```

---

## Decision Tree: Which Agent to Use?

```
Does the problem involve multiple sequential steps with dependencies?
├─ YES → Use @opera (multi-hop decomposition)
│         ├─ Need to verify logic? → Hand off to @reasoner
│         └─ Need systemic impact? → Hand off to @synthetic-analyst
│
└─ NO → Does the problem involve logical validation or contradiction detection?
        ├─ YES → Use @reasoner (RADD formal logic)
        │         └─ Need systemic context? → Hand off to @synthetic-analyst
        │
        └─ NO → Does the problem need holistic, system-wide analysis?
                ├─ YES → Use @synthetic-analyst (hermeneutic cycle)
                │         ├─ Need decomposition? → Hand off to @opera
                │         └─ Need logical verification? → Hand off to @reasoner
                │
                └─ NO → Clarify the problem; describe what you're trying to achieve
```

---

## Handoff Protocol

### @opera → @reasoner
**When:** OPERA produces a decomposition plan that needs formal verification

**Handoff Message:**
```
@reasoner: Please verify this decomposition for logical consistency.
Check for:
- Contradictions between sub-goals
- Circular dependencies
- Violated axioms or constraints
```

**Expected Output:** Validation report with flagged issues or approval

---

### @opera → @synthetic-analyst
**When:** OPERA produces a plan that needs systemic impact review

**Handoff Message:**
```
@synthetic-analyst: Please analyze the system-wide impact of this plan.
Consider:
- How this decomposition affects other agents
- Risks to system coherence
- Long-term maintenance implications
```

**Expected Output:** Diagnostic + Fix suggestions + Impact assessment

---

### @reasoner → @synthetic-analyst
**When:** RADD validation reveals contradictions between efficiency and safety

**Handoff Message:**
```
@synthetic-analyst: I identified a logical tension in this plan:
- Efficiency suggests [approach A]
- Safety suggests [approach B]
Please resolve the collision and recommend the best strategy.
```

**Expected Output:** Multi-strategy resolution matrix with recommendation

---

### Any Agent → @master-planner
**When:** Analysis is complete and ready for orchestration into action plan

**Handoff Message:**
```
@master-planner: Analysis complete. Here are the verified findings.
Please integrate into the master plan and decide on execution order.
```

**Expected Output:** Master plan with assignments to execution agents

---

## Real-World Example: Full Chain

### Scenario
**User:** "I want to add retrieval tools to all reasoning agents to make them faster."

### Execution

**Step 1: @synthetic-analyst analyzes the proposal holistically**
```
Deconstruction: Tools = data retrieval; Reasoning agents = pure cognition
Contextualization: System = agents have specialized roles; coupling matters
Collision: Efficiency (tools help) vs. Architecture (tools violate separation)
Conclusion: Problem! This violates separation of concerns principle.

Recommendation: Use handoff pattern instead (maintains architecture, enables efficiency).
```

**Step 2: @reasoner validates the recommendation**
```
Research: Current axiom = "Reasoning agents are pure cognition"
Abductive: Hypothesis = handoff pattern maintains both efficiency and architecture
Dialectic: Alternative (add tools) violates axiom; handoff respects it
Deductive: If axiom is true, then adding tools is invalid; handoff is valid

Conclusion: ✓ Recommend handoff pattern; reject direct tools
Confidence: HIGH (axiom is documented and enforced)
```

**Step 3: @opera decomposes the approved solution**
```
Sub-goal 1: Create handoff from reasoning agents to retrieval agents
Sub-goal 2: Update agent specifications to define handoff patterns
Sub-goal 3: Test handoff routing in orchestrator
Sub-goal 4: Verify performance is not degraded by indirection

Plan: [4-step decomposition with dependencies]
Trajectory: Each step's rationale documented
```

**Step 4: @master-planner orchestrates execution**
```
Phase 1: Planning (complete)
Phase 2: Implementation (begin with @meta-prompt-architect)
Phase 3: Testing (verify performance)
Phase 4: Deployment (if tests pass)
```

---

## Performance Characteristics

| Agent | Specialty | Speed | Depth | Best For |
|-------|-----------|-------|-------|----------|
| @opera | Multi-hop decomposition | Medium | Deep (4-6 hops) | Complex sequences |
| @reasoner | Logical verification | Fast | Medium (2-3 proofs) | Validation, contradiction detection |
| @synthetic-analyst | Systemic analysis | Slow | Deep (system-wide) | Impact assessment, strategy choice |

**Rule of Thumb:** Use @opera for complexity, @reasoner for rigor, @synthetic-analyst for safety.

---

## When NOT to Use Reasoning Agents

- **Simple queries** that don't need decomposition → Use direct LLM
- **Data retrieval only** (no reasoning) → Use @search or @github-repo agents
- **Execution tasks** → Use specialized execution agents (@meta-prompt-architect, etc.)
- **Quick facts** → Use retrieval agents, not reasoning agents

---

## Integration with Ecosystem

```
User Query
    ↓
    ├─→ [Routing Decision]
    │
    ├─→ Retrieval Needed? → @search / @github-repo
    │
    ├─→ Reasoning Needed?
    │   ├─→ Multi-hop? → @opera
    │   ├─→ Logical validation? → @reasoner
    │   └─→ Systemic analysis? → @synthetic-analyst
    │
    └─→ Execution Needed? → @meta-prompt-architect / @meta-prompt-optimizer
```

---

## Troubleshooting

### Problem: @opera produces a plan but @reasoner rejects it
**Solution:** Logic violation detected. Common issues:
- Circular dependencies in plan
- Sub-goal contradicts established axiom
- Unrealistic assumptions about information sufficiency

→ **Action:** Have @synthetic-analyst find resolution strategy (phased approach? alternative decomposition?)

### Problem: @synthetic-analyst identifies system impact but can't solve it
**Solution:** Collision is too complex for two-way resolution

→ **Action:** Have @opera decompose the collision into smaller pieces; @reasoner validate each piece

### Problem: All three agents agree, but implementation fails
**Solution:** Reasoning was sound but execution environment differs from assumptions

→ **Action:** Document the discrepancy; update axioms or assumptions in reasoning-framework.instructions.md

---

## Best Practices

1. **Use agents in their specialty** — Don't force @reasoner to decompose complex problems
2. **Chain handoffs logically** — Plan → Validate → Impact → Orchestrate
3. **Provide context** — Each handoff should explain what you need from the next agent
4. **Document assumptions** — Reasoning quality depends on valid premises
5. **Verify axioms** — If agents disagree, check if they're using different axioms
6. **Escalate conflicts** — If two agents contradict, escalate to @synthetic-analyst for resolution

---

## Summary

- **@opera** = Decomposition expert (sequential reasoning)
- **@reasoner** = Logic expert (formal verification)
- **@synthetic-analyst** = Holistic expert (system-wide impact)

Together, they provide decomposition → validation → integration, enabling confident decision-making on complex problems.
