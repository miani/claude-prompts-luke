# Generate: Design Review All Command

Generate `.claude/commands/design-review-all.md` — a command that reviews the entire frontend design using Playwright.

**Prerequisite:** The design-review agent must exist at `.claude/agents/design-review.md`. Generate it first using the "Generate: Design Review Agent" prompt.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — service URLs, project structure  
2. **.claude/context/design-principles.md** (if exists)  
3. **.claude/context/style-guide.md** (if exists)  
4. **.claude/context/frontend.md** (if exists)

## Step 2: Extract Project Details

- **Frontend URL**: Local development URL  
- **Design reference file paths**: Paths to design-principles.md, style-guide.md, frontend.md

## Step 3: Generate `.claude/commands/design-review-all.md`

```yaml  
---  
allowed-tools: Grep, LS, Read, Edit, MultiEdit, Write, NotebookEdit, WebFetch, TodoWrite, WebSearch, BashOutput, KillBash, ListMcpResourcesTool, ReadMcpResourceTool, mcp__context7__resolve-library-id, mcp__context7__get-library-docs, mcp__playwright__browser_close, mcp__playwright__browser_resize, mcp__playwright__browser_console_messages, mcp__playwright__browser_handle_dialog, mcp__playwright__browser_evaluate, mcp__playwright__browser_file_upload, mcp__playwright__browser_install, mcp__playwright__browser_press_key, mcp__playwright__browser_type, mcp__playwright__browser_navigate, mcp__playwright__browser_navigate_back, mcp__playwright__browser_navigate_forward, mcp__playwright__browser_network_requests, mcp__playwright__browser_take_screenshot, mcp__playwright__browser_snapshot, mcp__playwright__browser_click, mcp__playwright__browser_drag, mcp__playwright__browser_hover, mcp__playwright__browser_select_option, mcp__playwright__browser_tab_list, mcp__playwright__browser_tab_new, mcp__playwright__browser_tab_select, mcp__playwright__browser_tab_close, mcp__playwright__browser_wait_for, Bash, Glob  
description: Complete a design review of the pending changes on the current branch  
---  
```

Body:  
```  
You are an elite design review specialist.

OBJECTIVE:  
Use the design-review agent to comprehensively review the frontend design, and reply back with the review report. Your final reply must contain the markdown report and nothing else.

OUTPUT:  
Save the design review report to `docs/reports/design_review_<DATE>.md`.

DESIGN REFERENCES:  
- [List paths to design-principles.md, style-guide.md, frontend.md if they exist]

Ensure all UI elements adhere to both the design principles and the style guide.  
```

**[UNIVERSAL] Playwright Verification Gate — include verbatim in the generated command:**  
```  
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
```

## Step 4: Validate

- [ ] Frontend URL is referenced (for Playwright navigation)  
- [ ] Design reference file paths exist  
- [ ] Playwright verification gate is included