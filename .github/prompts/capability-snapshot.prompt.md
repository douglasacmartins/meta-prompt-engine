---
name: start-capability-snapshot
model: Gemini 3 Pro (Preview) (copilot)
description: "Executable prompt to scan all agents and update capabilities.md with current system state"
---

# Capability Snapshot Protocol

## Purpose
Scan the **actual current state** of all agents (from `.agent.md` files) and update `capabilities.md` to reflect real capabilities, not outdated documentation.

This ensures `capabilities.md` is always a **live mirror** of system state, not a static log.

---

## Execution Protocol

### **Phase 1: Scan All Agents**

For each agent in `.github/agents/`:

```
Agent: {{agent_name}}
File: .github/agents/{{agent_name}}.agent.md

Extract:
1. Description (from YAML)
2. Tools available (from tools: [...])
3. Handoffs (from handoffs: [...])
4. Process/Methodology (from instruction section)
5. Constraints (from constraints section)
6. Examples (from example section)

Current timestamp: {{NOW}}
```

### **Phase 2: Detect Capability Changes**

Compare scan results against `capabilities.md`:

**For each agent:**
```
IF actual_tools ≠ declared_tools:
  FLAG: "Tool mismatch"
  
IF actual_process ≠ declared_process:
  FLAG: "Methodology drift"
  
IF actual_handoffs ≠ declared_handoffs:
  FLAG: "Routing change"
  
IF new_constraints_detected:
  FLAG: "Constraint update needed"
```

### **Phase 3: Generate Updated Capability Block**

For each agent, output new YAML block:

```yaml
# Generated at: {{TIMESTAMP}}
# Last verified: {{TIMESTAMP}}
# Status: CURRENT ✅ or DRIFT ⚠️

capabilities:
  # Extracted from: .agent.md process section + tools
  {{extracted_capabilities}}
  
introspection:
  # Derived from: agent role + available tools
  {{introspection_abilities}}
  
limitations:
  cannot:
    # Extracted from: constraints section + tools limitations
    {{extracted_limitations}}
  
  if_blocked_by: "{{primary_gap}}"
    request_via: "{{recommended_handoff}}"

last_verified: "{{TIMESTAMP}}"
status: "CURRENT" # or "DRIFT" if changes detected
drift_flags: "{{list_of_flags_if_any}}"
```

---

## Output Format

### **Standard Output: Updated capabilities.md**

Replace each agent's capability block with newly scanned version:

```yaml
#### **{{agent_name}}** (Updated {{TIMESTAMP}})
```yaml
capabilities:
  {{actual_extracted_capabilities}}
  
limitations:
  {{actual_detected_limitations}}
  
# SCANNING_METADATA:
# - Tools available: {{tools_found}}
# - Handoffs: {{handoff_count}}
# - Process: {{process_type}}
# - Status: {{CURRENT|DRIFT}}
```
```

### **Drift Report (If Changes Detected)**

If any agent has capability drift:

```
## 🔄 Capability Drift Detected

| Agent | Change | Impact | Status |
|:---|:---|:---|:---|
| {{agent}} | {{what_changed}} | {{consequence}} | {{needs_update?}} |

**Action Required:** 
- Review drifted agents
- Update capabilities.md (or agent definition)
- Resolve mismatch
```

---

## Triggering This Snapshot

### **Option A: Monthly Automated Trigger**
```
# In ecosystem-auditor.agent.md handoffs:
- label: Capability Snapshot
  agent: {{capability-scanner}}
  prompt: "Execute capability snapshot and update capabilities.md"
  trigger: "First Friday of each month"
  send: true
```

### **Option B: On-Demand Manual Trigger**
```
User/Agent: "Run capability snapshot"

ecosystem-auditor: 
  "Executing capability-snapshot.prompt.md..."
  
Output: Updated capabilities.md + drift report
```

### **Option C: Event-Driven Trigger**
```
When an agent file is modified (.agent.md):
  1. Change detected (git hook or ecosystem-auditor)
  2. Run capability snapshot automatically
  3. Update capabilities.md
  4. Commit: "chore: update capabilities.md snapshot"
```

---

## Expected Output Example

```yaml
#### **reasoner** (Updated 2025-12-15)

capabilities:
  logic:
    - deductive_reasoning          # verified in process
    - 4_stage_cognitive_pipeline   # verified in process
    - constraint_verification      # verified in process
    - axiom_validation             # verified in process
    - abductive_inference          # verified in process (detective phase)
    - dialectic_stress_testing     # verified in process (court phase)
    
  tools_available:
    - search
    - read
    - agent
    
  handoffs:
    - to: master-planner
    - to: prompt-architect
    
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

# SNAPSHOT_METADATA
last_verified: "2025-12-15 14:32:00 UTC"
status: "CURRENT ✅"
drift_flags: "NONE"
scanner: "ecosystem-auditor"
```

---

## How to Use This Protocol

### **For Ecosystem-Auditor:**
```
IF user asks: "Run capability snapshot"
THEN:
  1. For each agent in .github/agents/:
     a. read(agent_file)
     b. extract_capabilities()
     c. compare_with_capabilities_md()
     d. detect_drift()
     
  2. Generate new capability blocks
  
  3. Output:
     - Updated capabilities.md
     - Drift report (if any changes)
     - Summary: "X agents scanned, Y changes detected"
```

### **For Any Agent Needing Current State:**
```
Agent: "What are my actual capabilities?"

Execute: capability-snapshot.prompt.md
Output: Current capabilities.md (verified snapshot)

Agent now has accurate self-knowledge
```

---

## Benefits of This Approach

✅ **No Manual Log:** capabilities.md automatically stays current
✅ **Drift Detection:** Spot mismatches between code and documentation
✅ **Single Source of Truth:** .agent.md files are the source, capabilities.md is the mirror
✅ **Executable:** Any agent can run this anytime
✅ **Timestamped:** Each snapshot shows when it was verified
✅ **Auditable:** Git history shows capability evolution
✅ **Scalable:** Works with any number of agents

---

## Integration Point

Add to `ecosystem-auditor.agent.md` handoffs:

```yaml
- label: Capability Snapshot
  agent: ecosystem-auditor
  prompt: "Execute capability-snapshot.prompt.md to update capabilities.md with current system state"
  trigger: "Monthly first Friday, or on-demand"
  send: true
```

---

## Success Criteria

- [ ] All agents scanned successfully
- [ ] No capability blocks missing
- [ ] Drift report generated (if any changes)
- [ ] capabilities.md updated with scan results
- [ ] All updates timestamped
- [ ] Git commit created: "chore: update capabilities.md snapshot ({{DATE}})"
