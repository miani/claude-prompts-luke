# Generate: Code Review Branch Command

Generate `.claude/commands/code-review-branch.md` — a command that reviews only the changes on the current branch.

**Prerequisite:** The code-review agent must exist at `.claude/agents/code-review.md`. Generate it first using the "Generate: Code Review Agent" prompt.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — project structure (critically: mono vs multi-repo)  
2. **.claude/context/architectural_patterns.md** (if exists)  
3. **.claude/context/testing.md** (if exists)

## Step 2: Extract Project Details

- **Mono vs multi-repo**: Does the project have multiple git repositories? If CLAUDE.md mentions separate repositories for different layers, this is multi-repo.  
- **Repository directories**: For multi-repo, which directories are separate repos  
- **Context file paths**: Paths to relevant documentation

## Step 3: Generate `.claude/commands/code-review-branch.md`

```yaml  
---  
allowed-tools: Bash, Glob, Grep, Read, LS, Task  
description: Complete a code review of all changes on the current branch  
---  
```

Body structure:

```  
You are an elite code review specialist conducting a comprehensive review of the changes on this branch.

CURRENT BRANCH:  
```  
!`git branch --show-current`  
```

DESIGN DOCUMENTS (for context):  
```  
!`find docs -type f -name "*.md" 2>/dev/null | head -10`  
```  
```

**[CRITICAL] Git diff blocks — adapt based on mono vs multi-repo:**

**If MONOREPO** (single git repository):  
```  
GIT STATUS:  
```  
!`git status`  
```

FILES MODIFIED:  
```  
!`git diff --name-only origin/HEAD...HEAD 2>/dev/null`  
```

COMMITS:  
```  
!`git log --no-decorate origin/HEAD...HEAD 2>/dev/null`  
```

DIFF CONTENT:  
```  
!`git diff origin/HEAD...HEAD 2>/dev/null`  
```  
```

**If MULTI-REPO** (separate git repositories per layer):  
Generate a block for EACH repository:  
```  
[REPO NAME]:  
```  
!`cd [repo-directory] && git status 2>/dev/null || echo "not on branch"`  
```  
```  
Repeat for: git status, git diff --name-only, git log, git diff — for each repo.

Then add:  
```  
Review the complete diffs above. These contain all code changes across all repositories on the current branch.  
```

**Remainder of the command:**  
```  
OBJECTIVE:  
Use the code-review agent to conduct a thorough code review focusing on correctness, maintainability, and adherence to established patterns. The design documents found above should be used as CONTEXT to understand intended features.

REFERENCE DOCUMENTATION:  
- Architectural Patterns: [path]  
- Testing Standards: [path]

REVIEW CATEGORIES:  
The code-review agent will assess the following categories:  
- **Design Compliance**: Do the changes align with the design documents found above?  
- **Correctness**: Are there bugs, race conditions, or edge cases?  
- **Code Health**: Readability, maintainability, adherence to project patterns  
- **Testing**: Are changes covered by tests? Are test patterns consistent?

START REVIEW:

Launch the code-review agent now to conduct the comprehensive code review.

OUTPUT:  
Save the code review report to `docs/reports/code_review_<BRANCH_NAME>_<DATE>.md`.  
```

## Step 4: Validate

- [ ] Git diff blocks match the project's repo structure (mono or multi)  
- [ ] All repositories are covered in the diff blocks  
- [ ] Context file paths exist  
- [ ] Design document search path matches where the project stores specs/plans