# Generate: Security Review Branch Command

Generate `.claude/commands/security-review-branch.md` — a command that reviews only branch changes for security vulnerabilities.

**Prerequisite:** The security-review agent must exist at `.claude/agents/security-review.md`. Generate it first using the "Generate: Security Review Agent" prompt.

## Step 1: Read Project Context

Read **CLAUDE.md** for project structure (mono vs multi-repo).

## Step 2: Extract Project Details

- **Mono vs multi-repo**: Same detection as other branch commands  
- **Repository directories**: For multi-repo, which directories are separate repos

## Step 3: Generate `.claude/commands/security-review-branch.md`

```yaml  
---  
allowed-tools: Bash, Glob, Grep, Read, LS, Task  
description: Complete a security review of all changes on the current branch  
---  
```

Body structure — same pattern as code-review-branch:

1. Current branch name via `!` syntax  
2. Design document discovery  
3. Git status/diff/log/diff-content blocks (mono or multi-repo)  
4. Objective scoped to branch changes:  
```  
OBJECTIVE:  
Perform a security-focused code review of the changes on this branch. Focus ONLY on security implications newly added by this branch. Do not comment on existing security concerns.  
```  
5. 3-step analysis pattern (same as security-review-all)  
6. Output path: `docs/reports/security_review_<BRANCH_NAME>_<DATE>.md`

## Step 4: Validate

- [ ] Git diff blocks match repo structure  
- [ ] Objective is scoped to branch changes only  
- [ ] 3-step analysis pattern preserved