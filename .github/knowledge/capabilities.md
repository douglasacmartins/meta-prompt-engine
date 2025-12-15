# Ecosystem Capabilities Registry

This file declares the **current capabilities** of each agent and defines **self-improvement protocols**.

Machine-parseable format: Agents can introspect this file to understand their own and others' boundaries.

**Generated at:** 2025-12-15 16:45:00 UTC  
**Last verified:** 2025-12-15 16:45:00 UTC  
**Status:** CURRENT ✅ (Post-Consolidation with Domain Suffixes)

---

## Capability Hierarchy

### **Core Domain (4 agents)**

#### **master-planner.core** (Updated 2025-12-15)
```yaml
capabilities:
  reasoning:
    - strategic_planning
    - system_architecture
    - o_p_e_r_a_framework
    - tree_of_thoughts_decomposition
  tools_available:
    - vscode/extensions
    - vscode/vscodeAPI
    - read
    - edit
    - search
    - web
    - agent
    - todo
  handoffs:
    - to: synthetic-analyst
    - to: meta-prompt-architect
    - to: reasoner
    - to: meta-prompter
    - to: meta-specialist-factory
    - to: meta-prompt-critic
    - to: ecosystem-auditor
    - to: meta-lifecycle-manager
    - to: meta-prompt-optimizer
  introspection:
    - can_analyze_system_state
    - can_request_improvements
    - can_route_to_specialists
    - can_orchestrate_complex_planning
limitations:
  cannot:
    - execute_code_directly
    - modify_agent_files_directly
    - deploy_without_critic_approval
  if_blocked_by: "insufficient_capability"
    request_via: "@meta-prompt-critic with improvement_request"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **ecosystem-orchestrator.core** (Updated 2025-12-15)
```yaml
capabilities:
  routing:
    - intent_classification
    - agent_selection_logic
    - request_delegation
    - intelligent_workflow_management
  tools_available:
    - read
    - search
    - agent
    - todo
    - web/fetch
  handoffs:
    - to: master-planner
    - to: ecosystem-auditor
    - to: synthetic-analyst
    - to: reasoner
    - to: meta-prompter
    - to: meta-prompt-architect
    - to: meta-prompt-critic
    - to: meta-specialist-factory
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

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **reasoner.core** (Updated 2025-12-15)
```yaml
capabilities:
  logic:
    - deductive_reasoning
    - 4_stage_cognitive_pipeline
    - constraint_verification
    - axiom_validation
    - abductive_inference
    - dialectic_stress_testing
  tools_available:
    - search
    - agent
    - read
    - todo
  handoffs:
    - to: master-planner
    - to: meta-prompt-architect
  introspection:
    - can_validate_proposed_changes
    - can_certify_logic_correctness
    - can_detect_logical_contradictions
limitations:
  cannot:
    - design_new_structures
    - modify_agent_files
    - execute_code
  if_blocked_by: "structural_reasoning_needed"
    request_via: "@meta-prompter for abstract syntax design"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **synthetic-analyst.core** (Updated 2025-12-15)
```yaml
capabilities:
  analysis:
    - systemic_decomposition
    - root_cause_analysis
    - synthesis_and_contextualization
    - hermeneutic_cycle
    - deconstruction_integration
  tools_available:
    - todo
    - search
  handoffs:
    - to: master-planner
  introspection:
    - can_identify_system_bottlenecks
    - can_propose_architectural_improvements
    - can_resolve_analytical_tensions
limitations:
  cannot:
    - implement_fixes_directly
    - execute_code
    - modify_system_files
  if_blocked_by: "incomplete_analysis_depth"
    request_via: "@reasoner for deeper verification"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

### **Creation Domain (3 agents)**

#### **meta-prompt-architect.creation** (Updated 2025-12-15)
```yaml
capabilities:
  design:
    - agent_scaffolding
    - prompt_generation
    - vs_code_configuration
    - classification_and_scaffolding
  tools_available:
    - vscode/vscodeAPI
    - execute/getTerminalOutput
    - execute/runInTerminal
    - read/readFile
    - edit/createFile
    - edit/editFiles
    - search
    - web
    - agent
    - todo
  handoffs:
    - to: meta-prompt-critic
  introspection:
    - can_create_new_agents
    - can_design_prompts
    - can_validate_handoff_targets
limitations:
  cannot:
    - create_experimental_agents
    - skip_validation_process
  if_blocked_by: "insufficient_design_context"
    request_via: "@meta-prompter for structural requirements"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **meta-prompt-optimizer.creation** (Updated 2025-12-15)
```yaml
capabilities:
  optimization:
    - prompt_refinement
    - template_evolution
    - continuous_improvement
    - enhanced_optimization_opera
    - dual_mode_optimization
  tools_available:
    - execute
    - read/readFile
    - edit/createFile
    - edit/editFiles
    - search
    - agent
    - todo
  handoffs:
    - to: meta-prompt-critic
    - to: ecosystem-orchestrator
    - to: meta-lifecycle-manager
  introspection:
    - can_improve_prompt_performance
    - can_evolve_templates
    - can_optimize_framework_effectiveness
limitations:
  cannot:
    - deploy_without_validation
    - modify_core_framework_without_approval
  if_blocked_by: "optimization_complexity"
    request_via: "@meta-prompt-critic for validation guidance"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "ENHANCED - absorbed template optimization capabilities"
```

#### **meta-specialist-factory.creation** (Updated 2025-12-15)
```yaml
capabilities:
  generation:
    - dynamic_agent_creation
    - novel_domain_specialization
    - template_based_generation
    - mpagf_pipeline_execution
  tools_available:
    - read
    - search
    - agent
    - todo
  handoffs:
    - to: meta-prompt-critic
    - to: ecosystem-orchestrator
    - to: master-planner
  introspection:
    - can_analyze_specialization_requirements
    - can_generate_agent_definitions
    - can_route_for_validation
limitations:
  cannot:
    - deploy_agents_without_validation
    - execute_agent_logic_directly
  if_blocked_by: "complex_multi_agent_requirements"
    request_via: "@master-planner for multi-agent architecture"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

### **Quality Domain (2 agents)**

#### **meta-prompt-critic.quality** (Updated 2025-12-15)
```yaml
capabilities:
  validation:
    - enhanced_comprehensive_validation
    - safety_architecture_consensus_async
    - multi_mode_validation_processing
    - constitutional_compliance
    - safety_security_validation
    - cognitive_quality_assessment
  tools_available:
    - read/readFile
    - search
    - agent
    - todo
  handoffs:
    - to: meta-prompt-architect
    - to: master-planner
    - to: meta-prompt-optimizer
    - to: ecosystem-orchestrator
  introspection:
    - can_validate_comprehensive_systems
    - can_process_consensus_validation
    - can_handle_async_validation_queues
limitations:
  cannot:
    - bypass_safety_standards
    - approve_without_proper_validation
  if_blocked_by: "validation_complexity_exceeds_capability"
    request_via: "@master-planner for strategic validation review"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "ENHANCED - consolidated 5 validation agents"
```

#### **pattern-validator.quality** (Updated 2025-12-15)
```yaml
capabilities:
  validation:
    - pattern_specification_validation
    - governance_compliance_checking
    - file_structure_validation
    - nlg_driven_reasoning
  tools_available:
    - read/readFile
    - search
  handoffs:
    - to: meta-prompt-critic
    - to: synthetic-analyst
    - to: ecosystem-orchestrator
    - to: meta-prompt-optimizer
    - to: ecosystem-orchestrator
  introspection:
    - can_validate_file_patterns
    - can_detect_governance_conflicts
    - can_suggest_consolidation_opportunities
limitations:
  cannot:
    - modify_files_directly
    - override_governance_standards
  if_blocked_by: "complex_governance_analysis_needed"
    request_via: "@synthetic-analyst for deep governance analysis"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

### **Infrastructure Domain (3 agents)**

#### **ecosystem-auditor.infrastructure** (Updated 2025-12-15)
```yaml
capabilities:
  auditing:
    - three_lens_audit_framework
    - structural_cognitive_workflow_analysis
    - ecosystem_health_monitoring
    - performance_coherence_optimization
  tools_available:
    - read/readFile
    - search
    - edit/editFiles
    - agent
    - todo
  handoffs:
    - to: synthetic-analyst
    - to: handoff-optimizer
    - to: meta-prompt-optimizer
    - to: master-planner
  introspection:
    - can_monitor_ecosystem_health
    - can_detect_system_bottlenecks
    - can_analyze_agent_performance
limitations:
  cannot:
    - modify_agents_directly
    - implement_optimizations_without_approval
  if_blocked_by: "ecosystem_complexity_exceeds_analysis_capability"
    request_via: "@synthetic-analyst for deep system analysis"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **conflict-detection.infrastructure** (Updated 2025-12-15)
```yaml
capabilities:
  conflict_detection:
    - instruction_file_conflict_analysis
    - pre_deployment_gatekeeper
    - opera_framework_conflict_detection
    - contradiction_duplication_detection
  tools_available:
    - read/readFile
  handoffs:
    - to: meta-prompt-critic
    - to: ecosystem-orchestrator
  introspection:
    - can_detect_instruction_conflicts
    - can_prevent_system_incoherence
    - can_analyze_precedence_violations
limitations:
  cannot:
    - resolve_conflicts_directly
    - modify_instruction_files
  if_blocked_by: "complex_conflict_resolution_needed"
    request_via: "@meta-prompt-critic for conflict resolution guidance"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **meta-lifecycle-manager.infrastructure** (Updated 2025-12-15)
```yaml
capabilities:
  lifecycle_management:
    - agent_promotion_evaluation
    - performance_monitoring
    - unified_operations_system
    - temporary_permanent_agent_management
    - real_time_performance_intelligence
  tools_available:
    - read/readFile
    - search
    - agent
    - todo
  handoffs:
    - to: ecosystem-orchestrator
    - to: meta-specialist-factory
    - to: ecosystem-auditor
    - to: meta-prompt-optimizer
    - to: master-planner
  introspection:
    - can_evaluate_agent_promotion
    - can_monitor_system_performance
    - can_detect_performance_bottlenecks
limitations:
  cannot:
    - promote_agents_without_criteria_validation
    - modify_system_performance_directly
  if_blocked_by: "performance_analysis_complexity_exceeds_capability"
    request_via: "@ecosystem-auditor for comprehensive system analysis"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "ENHANCED - absorbed performance monitoring capabilities"
```

### **Workflow Domain (2 agents)**

#### **handoff-optimizer.workflow** (Updated 2025-12-15)
```yaml
capabilities:
  workflow_optimization:
    - handoff_validation_optimization
    - agent_transition_design
    - workflow_auditing
    - recursion_prevention
  tools_available:
    - vscode/vscodeAPI
    - read/readFile
    - edit
    - search
    - web
    - agent
    - todo
  handoffs:
    - to: master-planner
  introspection:
    - can_optimize_agent_workflows
    - can_detect_handoff_issues
    - can_prevent_circular_routing
limitations:
  cannot:
    - create_hypothetical_agent_targets
    - bypass_handoff_validation_rules
  if_blocked_by: "workflow_complexity_exceeds_optimization_capability"
    request_via: "@master-planner for comprehensive workflow architecture"

last_verified: "2025-12-15 16:45:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
```

#### **meta-prompter.workflow** (Updated 2025-12-15)
```yaml
capabilities:
  structure:
    - abstract_syntax_definition
    - structural_pattern_design
    - meta_prompting_methodology
    - four_phase_meta_protocol
  tools_available:
    - vscode/vscodeAPI
    - read/readFile
    - edit/createFile
    - edit/editFiles
    - execute/runInTerminal
    - search
    - agent
    - todo
  handoffs:
    - to: master-planner
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

#### **conflict-detection** (NEW - Phase 3C)
```yaml
capabilities:
  detection:
    - contradiction_detection
    - duplication_detection
    - precedence_violation_detection
    - scope_overlap_detection
    - dependency_loop_detection
  analysis:
    - conflict_severity_assessment
    - affected_file_mapping
    - resolution_recommendation_generation
  integration:
    - github_pr_commenting
    - merge_blocking
    - structured_reporting
  introspection:
    - can_measure_conflict_coverage
    - can_trigger_deployment_blocking
limitations:
  cannot:
    - fix_conflicts_automatically
    - approve_deployments
    - modify_instruction_files_directly
  if_blocked_by: "conflict_resolution_ambiguity"
    request_via: "@prompt-critic for conflict resolution guidance"
```

#### **specialist-factory** (NEW - Phase 3D)
```yaml
capabilities:
  problem_analysis:
    - problem_intent_decomposition
    - required_specialty_identification
    - skill_requirement_extraction
    - constraint_pattern_recognition
  
  agent_generation:
    - meta_prompt_based_agent_creation
    - agent_definition_scaffolding
    - capability_profile_extraction
    - limitation_inference
  
  quality_assurance:
    - specification_validation_request
    - safety_compliance_checking
    - dry_principle_enforcement
    - agent_design_standard_compliance
  
  lifecycle_management:
    - temporary_agent_instantiation
    - execution_routing
    - result_aggregation
    - agent_cleanup_and_logging
  
  introspection:
    - can_measure_specialization_accuracy
    - can_track_agent_reusability
    - can_identify_pattern_specialties
    - can_request_permanent_agent_creation

limitations:
  cannot:
    - execute_agent_logic_directly
    - approve_agents_for_deployment
    - modify_instruction_files
    - bypass_prompt_critic_validation
  
  if_blocked_by: "safety_concerns_or_validation_failure"
    request_via: "@prompt-critic for validation OR @master-planner for escalation"
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

## Instruction Layer Coverage

The ecosystem relies on 3-layer instruction architecture (per VS Code official specs).

### **Layer 1: Global Rules** (`.github/copilot-instructions.md`)
- O.P.E.R.A. framework binding all agents
- Universal safety constraints
- System self-improvement protocol
- Status: ✅ COMPLETE (9 sections, 245 lines)

### **Layer 2: Contextual Rules** (`.github/instructions/*.instructions.md`)

| File | Purpose | Lines | Status | Coverage |
|:---|:---|---:|:---|:---|
| **reasoning-framework.instructions.md** | O.P.E.R.A., 4 cognitive patterns | 148 | ✅ | All agents |
| **safety-standards.instructions.md** | Constraints, audit rules | 151 | ✅ | All agents |
| **agent-design.instructions.md** | Agent creation standards | 230 | ✅ | New agent design |
| **prompt-engineering.instructions.md** | Cognitive optimization | 287 | ✅ | All agents |
| **system-integrity.instructions.md** | DRY, handoff validation, performance | 487 | ✅ | All agents |
| **tool-integration.instructions.md** | Tool selection, batching, optimization | 247 | ✅ NEW | Tool usage agents |
| **error-recovery.instructions.md** | Failure handling, escalation | 375 | ✅ NEW | Error scenarios |

**Total Layer 2:** 1,925 lines consolidated methodology, 100% of shared patterns

### **Layer 3: Minimal Agents** (`.github/agents/*.agent.md`)
- 10 active agents, avg 40 lines each
- Pure identity + unique process (no methodology embedded)
- Auto-inherit rules from Layer 2 via `applyTo: "**/*.agent.md"`
- Status: ✅ COMPLETE (zero duplication, 100% referenced)

---

## Instruction Coverage Summary

**New in Phase 3 Enhancement (Dec 15, 2025):**

1. **tool-integration.instructions.md** (247 lines)
   - Tool decision matrix (7 tools analyzed)
   - Parallel execution patterns (3x performance gain)
   - Tool-specific optimization (semantic_search, grep_search, file_search, read_file)
   - Error handling for each tool
   - Query optimization checklist

2. **error-recovery.instructions.md** (375 lines)
   - 4-level degradation: Retry → Fallback → Escalate → Abort
   - 5 common failure scenarios (tool timeout, empty search, circular routing, token budget, constraint violation)
   - Escalation protocol with templates
   - Checkpoint & state recovery for multi-step operations
   - Recovery checklist

3. **system-integrity.instructions.md** (extended +80 lines to 487 total)
   - Token budget guidelines per agent type
   - Parallel execution patterns with performance metrics
   - Query optimization for semantic vs. grep vs. file search
   - Output size management (summarization, chunking)
   - Performance checklist

**Impact:**
- ✅ Agents have clear guidance on tool usage and error recovery
- ✅ Performance standards quantified (token budgets, parallelization)
- ✅ Error scenarios have documented recovery patterns
- ✅ System capability expanded from 5 to 7 instruction files
- ✅ Zero new duplication introduced (DRY compliance maintained at 98%)

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
