# Generate: Design Review Branch Command

Generate `.claude/commands/design-review-branch.md` — a command that reviews UI changes on the current branch using Playwright.

**Prerequisite:** The design-review agent must exist at `.claude/agents/design-review.md`. Generate it first using the "Generate: Design Review Agent" prompt.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — project structure (mono vs multi-repo), service URLs  
2. **.claude/context/design-principles.md** (if exists)  
3. **.claude/context/style-guide.md** (if exists)  
4. **.claude/context/frontend.md** (if exists)

## Step 2: Extract Project Details

- **Mono vs multi-repo**: Same detection as code-review-branch  
- **Frontend directory/repo**: Which directory contains the frontend code  
- **Frontend URL**: Local development URL  
- **Design reference file paths**: Paths to design docs

## Step 3: Generate `.claude/commands/design-review-branch.md`

Same frontmatter as design-review-all (full Playwright tool list).

Body structure:

```  
You are an elite design review specialist.

CURRENT BRANCH:  
```  
!`git branch --show-current`  
```

DESIGN DOCUMENTS (for context):  
```  
!`find docs -type f -name "*design*.md" 2>/dev/null | head -10`  
```  
```

**Git diff blocks:** Generate git status, files modified, commits, and diff blocks. For multi-repo, include at minimum the root repo and the frontend repo. For mono-repo, include a single set.

```  
OBJECTIVE:  
Use the design-review agent to comprehensively review the UI/UX implementation of the changes above using Playwright. Your final reply must contain the markdown report and nothing else.

OUTPUT:  
Save the design review report to `docs/reports/design_review_<BRANCH_NAME>_<DATE>.md`.

DESIGN REFERENCES:  
- [paths to design docs]

CRITICAL REQUIREMENT - PLAYWRIGHT VERIFICATION:

**BEFORE STARTING ANY REVIEW WORK**, the design-review agent MUST:

1. Immediately call `mcp__playwright__browser_install` to verify Playwright MCP is available  
2. If the tool call FAILS or returns an error, the agent MUST:  
   - STOP all work immediately  
   - Return this error message and NOTHING ELSE:

   ```  
   DESIGN REVIEW FAILED: Playwright MCP Server Not Available

   This design review requires the Playwright MCP server to be running.

   To fix:  
   1. Ensure the Playwright MCP server is configured in your Claude settings  
   2. Restart Claude Code if needed  
   3. Verify MCP servers are running with the appropriate permissions

   Cannot proceed with visual design review without browser automation tools.  
   ```

3. If Playwright is available, proceed with the review

This is a BLOCKING requirement - do NOT attempt code-based reviews as a fallback.

START REVIEW:  
Launch the design-review agent now to conduct the live UI/UX review.  
```

## Step 4: Validate

- [ ] Git diff blocks cover the frontend repository  
- [ ] Playwright verification gate is included  
- [ ] Design reference paths exist