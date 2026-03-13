# Generate: Code Review All Command

Generate `.claude/commands/code-review-all.md` — a thin command that reviews the entire codebase by dispatching the code-review agent.

**Prerequisite:** The code-review agent must exist at `.claude/agents/code-review.md`. Generate it first using the "Generate: Code Review Agent" prompt.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — tech stack, project structure  
2. **.claude/context/architectural_patterns.md** (if exists)  
3. **.claude/context/testing.md** (if exists)

## Step 2: Extract Project Details

- **Project layers**: Each distinct layer/service with its source directory  
- **Context file paths**: Paths to architectural_patterns.md, testing.md, and other relevant context files

## Step 3: Generate `.claude/commands/code-review-all.md`

The generated command should be **thin** (\~40-60 lines). All review methodology lives in the code-review agent.

```yaml  
---  
allowed-tools: Read, Glob, Grep, LS, Task  
description: Complete a code review of the entire codebase  
---  
```

Body structure:

```  
You are an elite code review specialist conducting a comprehensive review of this project.

OBJECTIVE:  
Conduct a thorough code review of the entire codebase, focusing on correctness, maintainability, architectural consistency, and adherence to established patterns.

REFERENCE DOCUMENTATION:  
- Architectural Patterns: [path to architectural_patterns.md]  
- Testing Standards: [path to testing.md]  
- [Any other relevant context files]

REVIEW SCOPE:

You will systematically review:  
1. **[Layer 1 Name]** (`[layer 1 directory]`)  
2. **[Layer 2 Name]** (`[layer 2 directory]`)  
3. ...

START ANALYSIS:

Begin your analysis now. Use sub-tasks to parallelize the review across layers:

1. Spawn a sub-task to review [Layer 1]  
2. Spawn a sub-task to review [Layer 2]  
3. ...  
N. Synthesize findings into the final report

OUTPUT:  
Save the code review report to `docs/reports/code_review_<DATE>.md`.  
```

## Step 4: Validate

- [ ] Every project layer is listed in the review scope  
- [ ] All referenced context file paths exist  
- [ ] Command references the code-review agent's methodology (via sub-tasks)  
- [ ] Report output path is specified