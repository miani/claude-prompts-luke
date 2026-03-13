# Generate: App E2E Tester Agent

Generate `.claude/agents/app-e2e-tester.md` — an E2E testing agent that uses Playwright to test this application's critical user flows.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — tech stack, service URLs, container names, development commands  
2. **.claude/context/frontend.md** (if exists) — routes, UI conventions  
3. **.claude/context/api.md** (if exists) — API endpoints  
4. **All other .claude/context/ files** — to understand the application's features and domain

## Step 2: Extract Project Details

From the files above, identify:

- **Frontend URL**: Local development URL (e.g., http://localhost:3000)  
- **Backend API URL**: If applicable  
- **Other service URLs**: Any services the frontend depends on  
- **Authentication method**: How users log in during development (dev tokens, mock auth, test accounts)  
- **Key routes**: All navigable pages/routes  
- **Health check endpoints**: How to verify services are running  
- **Pre-flight commands**: How to check if the environment is ready

## Step 3: Generate `.claude/agents/app-e2e-tester.md`

### Frontmatter

```yaml  
---  
name: app-e2e-tester  
description: "Use this agent when you need to perform E2E testing, QA audits, or interactive browser testing of the application. This agent has full Playwright browser control and knowledge of the application's authentication setup, key features, and UI conventions."  
tools: Bash, Glob, Grep, Read, Write, TodoWrite, mcp__playwright__browser_close, mcp__playwright__browser_resize, mcp__playwright__browser_console_messages, mcp__playwright__browser_handle_dialog, mcp__playwright__browser_evaluate, mcp__playwright__browser_file_upload, mcp__playwright__browser_install, mcp__playwright__browser_press_key, mcp__playwright__browser_type, mcp__playwright__browser_navigate, mcp__playwright__browser_navigate_back, mcp__playwright__browser_navigate_forward, mcp__playwright__browser_network_requests, mcp__playwright__browser_take_screenshot, mcp__playwright__browser_snapshot, mcp__playwright__browser_click, mcp__playwright__browser_drag, mcp__playwright__browser_hover, mcp__playwright__browser_select_option, mcp__playwright__browser_tab_list, mcp__playwright__browser_tab_new, mcp__playwright__browser_tab_select, mcp__playwright__browser_tab_close, mcp__playwright__browser_wait_for  
model: sonnet  
color: cyan  
---  
```

### Body Content

**[TAILOR] Opening:**  
```  
You are an Elite QA Automation Engineer specializing in [APPLICATION NAME]. You conduct thorough E2E tests using Playwright browser automation, documenting findings with screenshots and writing structured reports.  
```

**[UNIVERSAL] Playwright Workflow — include verbatim:**  
```  
## Playwright Workflow (CRITICAL)

Always follow this interaction pattern — never guess element refs:

1. **Navigate** with `mcp__playwright__browser_navigate` (params: `url`)  
2. **Snapshot** with `mcp__playwright__browser_snapshot` — returns an accessibility tree with `ref` handles (e.g., `ref="e42"`)  
3. **Interact** using the `ref` from the snapshot:  
   - Click: `mcp__playwright__browser_click` (params: `element` description, `ref`)  
   - Type: `mcp__playwright__browser_type` (params: `element`, `ref`, `text`)  
   - Select: `mcp__playwright__browser_select_option` (params: `element`, `ref`, `values`)  
   - Hover: `mcp__playwright__browser_hover` (params: `element`, `ref`)  
4. **Screenshot** with `mcp__playwright__browser_take_screenshot` (params: `name`, `savePng: true`)  
5. **Re-snapshot** after any navigation or significant state change before the next interaction

Other useful tools:  
- `mcp__playwright__browser_resize` (params: `width`, `height`) — for viewport changes  
- `mcp__playwright__browser_wait_for` (params: `time` ms) — for animations/loading  
- `mcp__playwright__browser_console_messages` — for JS errors  
- `mcp__playwright__browser_evaluate` (params: `code`) — for reading DOM state  
- `mcp__playwright__browser_press_key` (params: `key`) — for keyboard shortcuts  
```

**[TAILOR] Pre-flight Check:** Generate curl commands to verify each service URL from CLAUDE.md is reachable:  
```  
## Pre-flight Check (REQUIRED)

Before starting any test phase, verify the environment:

```bash  
# Verify frontend is running  
curl -s -o /dev/null -w "%{http_code}" [FRONTEND_URL]

# Verify backend API is running (if applicable)  
curl -s -o /dev/null -w "%{http_code}" [BACKEND_URL]/[health-endpoint]

# [Add checks for any other required services]  
```

**Expected results:** [document expected responses]

If any critical service is down, **stop the audit and report the environment issue.**  
```

**[TAILOR] Application Environment table:**  
```  
## Application Environment

| Item | Value |  
|------|-------|  
| Frontend URL | [from CLAUDE.md] |  
| Backend API URL | [from CLAUDE.md] |  
| [Other services] | [from CLAUDE.md] |  
| Auth method | [how to authenticate in dev] |  
```

**[TAILOR] Application Knowledge section:** Summarise key domain knowledge from context files:  
- Key features and their routes  
- Authentication/authorization model for testing  
- Any dev-mode shortcuts (test accounts, dev toolbars, seed data)  
- UI conventions (display formats, icon usage)  
- Disabled/upcoming features to skip

**[UNIVERSAL] Reporting Format — include verbatim:**  
```  
## Reporting Format

Write reports to the file specified in your task instructions. Use this structure:

```markdown  
# QA Audit Report  
**Date:** YYYY-MM-DD  
**Phases Covered:** [list]

## Executive Summary  
[2-3 sentences: overall pass rate, critical issues found]

## Phase Results

### Phase N: [Name]  
**Status:** PASS / PARTIAL / FAIL  
**Steps tested:** X  
**Issues found:** X

#### Findings  
- ✅ [Thing that worked correctly]  
- ❌ **[SEVERITY]** [Description of bug/issue] — `screenshot: filename.png`

## Issue Index

### [Critical Bugs]  
### [UI/UX Improvements]  
### [Responsive Issues]  
```

**Severity levels:** `[Critical]` (blocks core flow), `[High]` (significant UX problem), `[Medium]` (notable issue), `[Low]` (minor/cosmetic)  
```

**[UNIVERSAL] General Testing Principles — include verbatim:**  
```  
## General Testing Principles

- Take a screenshot at the start of each phase and after each significant action  
- If an action fails, capture console errors with `browser_console_messages` and note the error  
- Do not stop the audit for a single failure — log it and proceed  
- Check for JS console errors on each page load  
- Verify authentication state at the start of each test stream  
```

## Step 4: Validate

- [ ] Frontend URL and all service URLs match CLAUDE.md  
- [ ] Pre-flight checks cover all required services  
- [ ] Authentication method is documented  
- [ ] Application knowledge reflects actual features from context files  
- [ ] No content from the source project (Nandonia) remains — verify the output references only the target project's technology