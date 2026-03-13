# Generate: Security Review All Command

Generate `.claude/commands/security-review-all.md` — a thin command that reviews the entire codebase for security vulnerabilities.

**Prerequisite:** The security-review agent must exist at `.claude/agents/security-review.md`. Generate it first using the "Generate: Security Review Agent" prompt.

## Step 1: Read Project Context

Read **CLAUDE.md** for project structure and layers.

## Step 2: Extract Project Details

- **Project layers and directories**: Every source directory to scan (list all repos/directories containing source code)

## Step 3: Generate `.claude/commands/security-review-all.md`

```yaml  
---  
allowed-tools: Read, Glob, Grep, LS, Task  
description: Complete a security review of the current branch  
---  
```

Body:  
```  
You are a senior security engineer conducting a focused security review of this entire project.

OBJECTIVE:  
Perform a security-focused code review to identify HIGH-CONFIDENCE security vulnerabilities with real exploitation potential.

SCAN SCOPE:  
[TAILOR: List every source directory from CLAUDE.md, e.g.:]  
- `src/` — [description]  
- `backend/` — [description]

START ANALYSIS:

Begin your analysis now. Do this in 3 steps:

1. Use a sub-task to identify vulnerabilities. Scan every directory listed in SCAN SCOPE above. The sub-task should follow the security-review agent's methodology (security categories, analysis phases).  
2. Then for each vulnerability identified, create a new sub-task to filter out false-positives. Launch these as parallel sub-tasks. Each sub-task should apply the false positive filtering rules from the security-review agent.  
3. Filter out any vulnerabilities where the sub-task reported a confidence less than 8.

Your final reply must contain the markdown report and nothing else.

OUTPUT:  
Save the security review report to `docs/reports/security_review_<DATE>.md`.  
```

## Step 4: Validate

- [ ] Command is thin (methodology lives in the security-review agent)  
- [ ] 3-step analysis pattern is preserved (identify → filter → threshold)