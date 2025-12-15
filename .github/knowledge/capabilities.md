# Ecosystem Capabilities Registry

This file declares the **current capabilities** of each agent and defines **self-improvement protocols**.

Machine-parseable format: Agents can introspect this file to understand their own and others' boundaries.

---

## Capability Hierarchy

### **Tier 1: Core Reasoning Agents**

#### **master-planner**
```yaml
capabilities:
  reasoning:
    - strategic_planning
    - system_architecture
    - o_p_e_r_a_framework
    - tree_of_thoughts_decomposition
  introspection:
    - can_analyze_system_state
    - can_request_improvements
    - can_route_to_specialists
limitations:
  cannot:
    - execute_code_directly
    - modify_agent_files_directly
    - deploy_without_critic_approval
  if_blocked_by: "insufficient_capability"
    request_via: "@prompt-critic with improvement_request"
```

#### **ecosystem-orchestrator**
```yaml
capabilities:
  routing:
    - intent_classification
    - agent_selection_logic
    - request_delegation
  introspection:
    - can_measure_request_distribution
    - can_detect_unhandled_requests
    - can_trigger_improvement_loop
limitations:
  cannot:
    - execute_agent_work_directly
    - modify_routing_table_directly
  if_blocked_by: "routing_gap"
    request_via: "@meta-prompter for new routing pattern"
```

#### **synthetic-analyst**
```yaml
capabilities:
  analysis:
    - systemic_decomposition
    - root_cause_analysis
    - synthesis_and_contextualization
    - hermeneutic_cycle
  introspection:
    - can_identify_system_bottlenecks
    - can_propose_architectural_improvements
limitations:
  cannot:
    - implement_fixes_directly
  if_blocked_by: "incomplete_analysis_depth"
    request_via: "@reasoner for deeper verification"
```

#### **reasoner**
```yaml
capabilities:
  logic:
    - deductive_reasoning
    - 4_stage_cognitive_pipeline
    - constraint_verification
    - dialectic_stress_testing
  introspection:
    - can_validate_proposed_changes
    - can_certify_logic_correctness
limitations:
  cannot:
    - design_new_structures
  if_blocked_by: "structural_reasoning_needed"
    request_via: "@meta-prompter for abstract syntax design"
```

#### **meta-prompter**
```yaml
capabilities:
  structure:
    - abstract_syntax_definition
    - structural_pattern_design
    - meta_prompting_methodology
  introspection:
    - can_generate_new_reasoning_templates
    - can_abstract_solution_patterns
    - can_create_domain_specific_syntax
limitations:
  cannot:
    - implement_designs
    - critique_for_safety
  if_blocked_by: "implementation_needed"
    request_via: "@prompt-architect for structural scaffolding"
```

### **Tier 2: Workflow Agents**

#### **prompt-architect**
```yaml
capabilities:
  design:
    - agent_file_creation
    - prompt_template_scaffolding
    - o_p_e_r_a_structure_injection
    - handoff_graph_design
  introspection:
    - can_generate_new_agent_definitions
    - can_propose_workflow_structures
limitations:
  cannot:
    - validate_safety
    - optimize_performance
  if_blocked_by: "safety_concerns"
    request_via: "@prompt-critic for validation before deployment"
```

#### **prompt-critic**
```yaml
capabilities:
  audit:
    - constitutional_compliance_checking
    - safety_and_security_analysis
    - cognitive_quality_assessment
    - logic_validation_gating
  approval:
    - can_approve_deployments
    - can_request_modifications
    - can_route_improvements_to_optimizer
  introspection:
    - can_identify_quality_gaps
    - can_certify_readiness_for_production
limitations:
  cannot:
    - modify_drafts_directly
    - implement_optimizations
  if_blocked_by: "optimization_needed"
    request_via: "@prompt-optimizer with audit_report"
```

#### **prompt-optimizer**
```yaml
capabilities:
  optimization:
    - syntax_hygiene
    - cognitive_reinforcement
    - chain_of_thought_injection
    - few_shot_example_generation
    - variable_injection_{{handlebars}}
  introspection:
    - can_measure_prompt_quality_metrics
    - can_detect_missing_groundings
limitations:
  cannot:
    - validate_final_safety
  if_blocked_by: "safety_verification_needed"
    request_via: "@prompt-critic for final verification"
```

#### **handoff-optimizer**
```yaml
capabilities:
  workflow:
    - handoff_graph_analysis
    - agent_verification
    - routing_correctness_validation
    - recursion_detection
  introspection:
    - can_map_agent_connectivity
    - can_detect_unreachable_agents
    - can_identify_circular_dependencies
limitations:
  cannot:
    - redesign_agent_logic
  if_blocked_by: "structural_redesign_needed"
    request_via: "@meta-prompter for workflow abstraction"
```

#### **ecosystem-auditor**
```yaml
capabilities:
  audit:
    - three_lens_protocol (structure, cognition, workflow)
    - ecosystem_health_assessment
    - performance_bottleneck_detection
    - quality_recommendations
  introspection:
    - can_measure_system_coherence
    - can_identify_optimization_opportunities
    - can_trigger_targeted_improvements
limitations:
  cannot:
    - implement_fixes
  if_blocked_by: "requires_system_intervention"
    request_via: "@synthetic-analyst for root cause, then @master-planner for orchestration"
```

---

## Capability Snapshot Protocol (Live Updates)

**This file is kept current via automated snapshots, not manual logs.**

### How It Works

1. **Monthly/On-Demand Trigger:** ecosystem-auditor executes `capability-snapshot.prompt.md`
2. **Scanning:** Reads all `.agent.md` files to extract actual current capabilities
3. **Verification:** Compares actual capabilities against documented capabilities
4. **Update:** Replaces outdated capability blocks with verified scans
5. **Drift Detection:** Reports any mismatches between code and documentation
6. **Result:** capabilities.md becomes a **live mirror** of system state

### Execution

**Manual trigger:**
```
User: "Run capability snapshot"
ecosystem-auditor executes: .github/instructions/capability-snapshot.prompt.md
Output: Updated capabilities.md + drift report
```

**Automated trigger (optional):**
```
First Friday of each month:
  ecosystem-auditor automatically runs snapshot
  Git commit created: "chore: update capabilities.md snapshot"
```

### Last Snapshot

- **Date:** 2025-12-15 (Verified Scan)
- **Time:** 14:45 UTC
- **Scanner:** ecosystem-auditor (Capability Snapshot Protocol)
- **Agents Scanned:** 10/10 active agents
- **Changes Detected:** 0 drift flags
- **Status:** ✅ ALL CURRENT
- **Verification:** All tool lists, handoffs, processes match actual agent definitions

---

## Self-Improvement Protocol

### **Capacity Insufficient: What to Do**

1. **Recognize Limitation:**
   ```
   "I am {{agent_name}}.
    I need to accomplish: {{task}}.
    My current capabilities: {{current_caps}}.
    Missing capability: {{gap}}."
   ```

2. **Check Authority:**
   - Read this file (`capabilities.md`)
   - Verify you have `introspection: can_request_improvements`
   - Confirm gap is in your `if_blocked_by` section

3. **Compose Improvement Request:**
   ```yaml
   improvement_request:
     agent: {{your_agent_name}}
     current_limitation: "Cannot {{action}}"
     requested_enhancement: "Add {{instruction/example/tool}}"
     rationale: "Enables capability: {{capability}}"
     verification_method: "Test: {{test_case}}"
     estimated_impact: "{{metric_to_improve}}"
   ```

4. **Submit via Gated Channel:**
   ```
   @prompt-critic: Review this improvement request.
   - Is it safe? (No security risks, no hallucination)
   - Is it necessary? (Gap is real)
   - Is it well-formed? (Clear, testable)
   
   If approved: Route to @prompt-optimizer for polish,
   then deploy to .github/instructions/ or .github/agents/
   ```

5. **Track Change:**
   - Document improvement in this file (add to `improvements_log`)
   - Update agent's capabilities in next section
   - Measure impact: Did capability improve? Did performance increase?

---

## Snapshot Status & Drift History

| Date | Time | Agents Scanned | Changes Detected | Status | Scanner | Verification |
|:---|:---|---:|---:|:---|:---|:---|
| 2025-12-15 | 14:45 UTC | 10/10 | 0 | ✅ ALL CURRENT | ecosystem-auditor | Full scan complete |
| (Next snapshot) | TBD | TBD | TBD | TBD | ecosystem-auditor | Scheduled for 2026-01-17 (first Friday) |

**How to trigger next snapshot:**
- Manual: `User: "Run capability snapshot"`
- Automatic: First Friday of next month
- Git hook: On any `.agent.md` file modification

---

## System Constraints (Non-Negotiable)

## System Constraints (Non-Negotiable)

1. **Safety Gate:** All improvements must pass `@prompt-critic` validation
2. **Auditability:** All changes logged with rationale and impact
3. **Reversibility:** All changes tracked in git; rollback always possible
4. **Clarity:** No ambiguous improvements; each must have testable success criteria
5. **DRY:** Shared patterns in Knowledge Base; no instruction duplication

---

## Future Capability Roadmap

| Capability | Current | Target | Agent | Effort |
|:---|:---|:---|:---|:---|
| Code execution | ❌ | 🟡 Can run tests only | reasoner + executor (new) | Medium |
| Real-time metrics | ❌ | ✅ Measure improvements | ecosystem-auditor | Low |
| Automatic rollback | ❌ | ✅ On failure detection | handoff-optimizer | Medium |
| Multi-modal reasoning | ❌ | 🟡 Images/tables | meta-prompter | High |
| Cross-team collaboration | ✅ (basic) | 🟡 Parallel agents | ecosystem-orchestrator | Medium |
