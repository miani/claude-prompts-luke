# Generate: Performance Review Command

Generate `.claude/commands/performance-review.md` — an orchestrator command that runs a full-stack performance audit using parallel sub-tasks.

**Prerequisite:** The performance-review agent must exist at `.claude/agents/performance-review.md`. Generate it first using the "Generate: Performance Review Agent" prompt.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — tech stack, project structure, layers  
2. **.claude/context/architectural_patterns.md** (if exists)

## Step 2: Extract Project Details

- **Project layers and directories**: Each layer that needs its own performance analysis  
- **Database layer**: Whether there's a database to analyze separately  
- **Context file paths**: Paths to architectural patterns, domain docs

## Step 3: Generate `.claude/commands/performance-review.md`

```yaml  
---  
allowed-tools: Read, Glob, Grep, LS, Task, Bash  
description: Complete a performance review of the entire codebase  
---  
```

Body — the command is an **orchestrator** with this structure:

**[TAILOR] Opening:**  
```  
You are an elite performance optimization specialist conducting a comprehensive performance audit of this project.

OBJECTIVE:  
Conduct a thorough performance audit of the entire codebase, identifying bottlenecks, optimization opportunities, and performance anti-patterns across all layers.

REFERENCE DOCUMENTATION:  
- [paths to relevant context files]

REVIEW SCOPE:  
1. **[Layer 1]** (`[directory]`)  
2. **[Layer 2]** (`[directory]`)  
...  
N. **Database Schema** (`[migrations/entity directory]`) — if applicable  
```

**[UNIVERSAL] Context-efficient execution pattern — include verbatim:**  
```  
**IMPORTANT: Context-Efficient Execution Pattern**

To prevent "Prompt is too long" errors, use a **file-based intermediate output** strategy. Each subagent writes findings to a temporary file rather than returning large outputs directly. The final synthesis reads each file sequentially.  
```

**[UNIVERSAL] Phase 0 — Past review check:**  
```  
## Phase 0: Check Past Reviews

Before starting analysis, check for existing performance reviews:  
```bash  
ls -la docs/reports/performance_review_*.md  
```

If past reviews exist, read the most recent one. Any findings from past reviews that appear in the new analysis MUST be re-verified against current code before being included. Do NOT copy findings from past reviews — treat them only as leads that require fresh verification.  
```

**[TAILOR] Phase 1 — Parallel sub-task dispatch.** Generate one sub-task per project layer plus one for database (if applicable). Each sub-task should:  
- Analyze its assigned layer using the performance-review agent's methodology  
- **Verify every finding against actual code before reporting**  
- Write findings to its temp file (e.g., `/tmp/perf_[layer].md`)  
- Return only a brief confirmation

Include the verification rules in each sub-task prompt:  
```  
IMPORTANT: For every finding, verify it against the actual code. Before claiming something is missing or suboptimal, search for it. Include verification evidence (file:line checked) with each finding. Do NOT report unverified findings.  
```

**[UNIVERSAL] Phase 2 — Verification pass:**  
```  
## Phase 2: Verification Pass

After all subagent tasks complete but BEFORE synthesizing the report:

1. Read each temp file sequentially  
2. For any finding that lacks verification evidence, verify it yourself  
3. **Remove any finding that turns out to be a false positive**  
4. If a past review exists, cross-reference — any repeated finding must be re-verified  
```

**[UNIVERSAL] Phase 3 — Sequential report synthesis:**  
```  
## Phase 3: Sequential Report Synthesis

Build the final report ONE SECTION AT A TIME:

1. Create report header  
2. Read each temp file → Extract key findings → Append section to report  
3. Synthesize cross-layer analysis  
4. Add optimization roadmap prioritized by impact vs effort

**Critical**: Do NOT read all temp files at once. Process each sequentially to minimize context usage.  
```

**[UNIVERSAL] Phase 4 — Cleanup:**  
```  
## Phase 4: Cleanup

Delete temporary files after synthesis:  
```bash  
rm -f /tmp/perf_*.md  
```  
```

**Output:**  
```  
Save the final report to `docs/reports/performance_review_<DATE>.md`.  
```

## Step 4: Validate

- [ ] One sub-task per project layer (plus database if applicable)  
- [ ] Temp file names are unique per layer  
- [ ] Verification rules are included in sub-task prompts  
- [ ] Past review check is included  
- [ ] Sequential synthesis pattern is preserved