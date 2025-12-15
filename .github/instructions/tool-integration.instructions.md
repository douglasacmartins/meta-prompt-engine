---
name: Tool Integration Patterns
description: Guidance on optimal tool selection, batching, and error handling
applyTo: "**/*.agent.md"
---

# Tool Selection & Integration

Agents have access to specialized tools. This guide helps you choose the right tool for each task and use them efficiently.

---

## Tool Decision Matrix

When solving a task, select the tool based on your problem type:

| Problem Type | Best Tool(s) | When to Use | Speed | Accuracy |
|:---|:---|:---|:---|:---|
| **"Find exact filename"** | `file_search` | You know the exact pattern (e.g., `**/*.agent.md`) | ⚡ Fast | ✅ Perfect |
| **"Search specific file for keyword"** | `grep_search` | Pattern matching within a single file or folder | ⚡ Fast | ✅ High |
| **"Search semantic meaning across codebase"** | `semantic_search` | You need understanding, not just text match | 🐢 Slower | ✅ Excellent |
| **"Read file contents"** | `read_file` | You need exact code/documentation | ⚡ Fast | ✅ Perfect |
| **"Make code changes"** | `replace_string_in_file` | You need precise, validated edits | ⚡ Fast | ✅ Safe |
| **"Browse directory structure"** | `list_dir` | You need to understand folder layout | ⚡ Fast | ✅ Perfect |
| **"Complex multi-step edits"** | `multi_replace_string_in_file` | Multiple independent replacements needed | ⚡ Fast | ✅ Efficient |

---

## Parallel Execution (Batch Operations)

When multiple tool calls are **independent**, batch them into a single `<function_calls>` block.

### Why Batch?

**Without batching (Sequential):**
```
Call 1: read_file (file1.ts)       → Wait for response
Call 2: read_file (file2.ts)       → Wait for response
Call 3: read_file (file3.ts)       → Wait for response
Total Time: ~3 seconds (1 second per call + network latency)
```

**With batching (Parallel):**
```
Call 1: read_file (file1.ts)  }
Call 2: read_file (file2.ts)  } → All execute in parallel
Call 3: read_file (file3.ts)  }
Total Time: ~1 second (all parallel + network latency)
```

**Result:** 3x faster execution.

### Parallel Pattern

Use this pattern when tool calls don't depend on each other:

```yaml
<function_calls>
<invoke name="read_file">
  <parameter name="filePath">file1.ts</parameter>
  <parameter name="startLine">1</parameter>
  <parameter name="endLine">50</parameter>
</invoke>
<invoke name="read_file">
  <parameter name="filePath">file2.ts</parameter>
  <parameter name="startLine">1</parameter>
  <parameter name="endLine">50</parameter>
</invoke>
<invoke name="grep_search">
  <parameter name="query">function handleClick</parameter>
  <parameter name="isRegexp">false</parameter>
</invoke>
</function_calls>
```

Result: All 3 execute in parallel, 3x faster than sequential.

---

## Tool-Specific Optimization

### semantic_search (Most Expensive)

**Cost:** Slowest, highest token consumption  
**Quality:** Excellent understanding of meaning  
**When to use:** Understanding context, finding related code  
**Optimization:** Ask broad question once, not narrow questions repeatedly

❌ Bad:
```
semantic_search("agent design")
semantic_search("agent specialization")
semantic_search("agent classification")
```
(3 expensive calls)

✅ Good:
```
semantic_search("agent design, specialization, classification patterns")
```
(1 call with combined query)

---

### grep_search (Medium Cost)

**Cost:** Fast, pattern-based matching  
**Quality:** High precision for known patterns  
**When to use:** Find specific function names, keywords, patterns  
**Optimization:** Use regex patterns to narrow results upfront

❌ Bad:
```
grep_search("function")  # Matches everything
```
(Too broad, many results)

✅ Good:
```
grep_search("^export function", isRegexp=true)  # Only export functions
```
(Narrow results immediately)

---

### file_search (Fastest)

**Cost:** Instant, glob-based  
**Quality:** Perfect for known patterns  
**When to use:** Find files by name/pattern  
**Optimization:** Use specific glob patterns, not wildcards

❌ Bad:
```
file_search("*")  # Everything
```

✅ Good:
```
file_search("**/*.agent.md")  # Only agent files
```

---

### read_file (Balanced)

**Cost:** Fast, but token cost = file size  
**Quality:** Perfect accuracy  
**When to use:** When you need exact contents  
**Optimization:** Read large sections once, not many small reads

❌ Bad:
```
read_file(lines 1-50)     # Call 1
read_file(lines 50-100)   # Call 2
read_file(lines 100-150)  # Call 3
```

✅ Good:
```
read_file(lines 1-150)  # Single call, reads full section
```

---

## Error Handling for Tools

Each tool can fail. Handle gracefully:

### Tool Timeout

```
IF tool_call returns timeout:
  → Retry once with smaller scope (fewer files, shorter search)
  → If retry fails: Escalate to prompt-critic
```

Example:
```
semantic_search("broad query") → timeout
→ Retry with: semantic_search("narrower query")
→ If still timeout: Handoff to prompt-critic
```

### Search Returns Empty

```
IF search returns 0 results:
  → Broaden query (remove restrictive keywords)
  → Try different tool (grep_search instead of semantic_search)
  → If still nothing: Assume not in codebase, continue
```

### File Not Found

```
IF read_file returns "Invalid path":
  → Verify path is absolute (not relative)
  → Use file_search to find correct path
  → Retry read_file with correct path
```

---

## Query Optimization Checklist

Before executing tool calls:

- [ ] Tool choice matches problem type (use decision matrix)
- [ ] Independent calls are batched (parallel execution)
- [ ] Queries are specific, not overly broad
- [ ] File paths are absolute, not relative
- [ ] Regex patterns are correct (if using grep_search)
- [ ] Search scope is reasonable (not entire codebase for simple lookups)
- [ ] Fallback strategy defined (if primary tool fails)

---

## Integration Example

**Task:** Update 3 agent files after finding all references to a deprecated function.

**Approach:**
1. **Search Phase (Parallel):**
   ```
   grep_search("deprecated_function") on agent files
   ```

2. **Read Phase (Parallel):**
   ```
   read_file for all 3 matching agents
   ```

3. **Edit Phase (Batch):**
   ```
   multi_replace_string_in_file with all 3 replacements at once
   ```

**Result:** Efficient, minimal tool calls, maximum parallelization.

---

## Reference

- Related: reasoning-framework.instructions.md (O.P.E.R.A. framework)
- Related: system-integrity.instructions.md (Performance metrics)
- Related: error-recovery.instructions.md (Failure handling)