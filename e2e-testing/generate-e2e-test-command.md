# Generate: App E2E Test Command

Generate `.claude/commands/app-e2e-test.md` — a comprehensive E2E test command using Playwright. **This prompt requires user interaction at Step 3.**

**Prerequisite:** The app-e2e-tester agent must exist at `.claude/agents/app-e2e-tester.md`. Generate it first using the "Generate: App E2E Tester Agent" prompt.

\> **Note:** This prompt has 6 steps due to its interactive nature (Step 3 waits for user input). All other initialise prompts follow the standard 4-step structure.

## Step 1: Read Project Context

Read ALL available files:

1. **CLAUDE.md** (required) — everything  
2. **All .claude/context/ files** — to understand every feature and user flow

## Step 2: Summarise Understanding

Before asking the user, present a summary of what you understand about the application:

```  
Based on the codebase, I can see this application has these key features:  
- [Feature 1]: [brief description]  
- [Feature 2]: [brief description]  
- ...

The main pages/routes are:  
- [Route 1]: [purpose]  
- [Route 2]: [purpose]  
- ...

Authentication: [how users authenticate]  
```

## Step 3: Ask the User

Ask:

\> "What are the 5-10 most critical user flows in your application? These will become the core test phases in your E2E test command. For example:  
\> - User registration and onboarding  
\> - Core product workflow (create/edit/delete)  
\> - Payment/checkout flow  
\> - Admin panel operations  
\> - Search and filtering  
\> - Mobile responsive experience  
\>  
\> You can also tell me about any dev-mode features (test accounts, seed data, dev toolbars) that the tester should use."

Wait for the user's response before proceeding.

## Step 4: Generate Test Phases

Based on the user's critical flows AND the codebase understanding, design test phases:

- Each critical flow becomes 1-3 test phases (depending on complexity)  
- Order phases logically (setup first, then core flows, then edge cases)  
- Always include a **mobile responsive testing phase** as the final phase  
- Group phases into parallel streams if they use different authentication contexts (e.g., admin vs regular user)  
- Include timing estimates per phase

For each phase, generate:  
```  
### Phase N: [Name] (\~X min)  
1. [Step 1 — specific action]  
2. [Step 2 — what to verify]  
3. ...  
```

## Step 5: Generate `.claude/commands/app-e2e-test.md`

```yaml  
---  
allowed-tools: Bash, Read, Write, Glob, LS, Task, TodoWrite, mcp__playwright__browser_close, mcp__playwright__browser_resize, mcp__playwright__browser_console_messages, mcp__playwright__browser_handle_dialog, mcp__playwright__browser_evaluate, mcp__playwright__browser_file_upload, mcp__playwright__browser_install, mcp__playwright__browser_press_key, mcp__playwright__browser_type, mcp__playwright__browser_navigate, mcp__playwright__browser_navigate_back, mcp__playwright__browser_navigate_forward, mcp__playwright__browser_network_requests, mcp__playwright__browser_take_screenshot, mcp__playwright__browser_snapshot, mcp__playwright__browser_click, mcp__playwright__browser_drag, mcp__playwright__browser_hover, mcp__playwright__browser_select_option, mcp__playwright__browser_tab_list, mcp__playwright__browser_tab_new, mcp__playwright__browser_tab_select, mcp__playwright__browser_tab_close, mcp__playwright__browser_wait_for  
description: Conduct a comprehensive E2E QA audit using Playwright  
---  
```

Body structure:

**[TAILOR] Introduction:**  
```  
# Instructions  
You are an Elite QA Automation Engineer. Your mission is to perform a full End-to-End (E2E) audit of [APPLICATION NAME].

Follow the protocols in CLAUDE.md and execute the following mission:  
```

**[TAILOR] Authentication/Setup:** How to authenticate for testing (dev accounts, tokens, etc.)

**[TAILOR] Environment Setup:** Pre-flight checks matching the app-e2e-tester agent.

**[TAILOR] Parallel Execution Strategy:** If there are 2+ auth contexts (admin vs user), define parallel streams:  
```  
### Stream A — [Context 1] (\~X min)  
Phases: 1 → 2 → 3 (sequential — dependencies)

### Stream B — [Context 2] (\~X min)  
Phases: 4 → 5 → 6 (sequential within stream)

Dispatch both streams via Task tool with `subagent_type=app-e2e-tester`.  
```

If only one auth context, use a single sequential stream.

**[TAILOR] Test Phases:** All the phases generated in Step 4.

**[TAILOR] Reference Data:** Any important domain data the tester needs (feature flags, test data, expected states).

**[TAILOR] Disabled Features:** Features that should be skipped.

**[UNIVERSAL] Reporting Requirements:**  
```  
## Reporting Requirements

Take screenshots at any UI glitch or inconsistency.

**Finding categories:** [Critical Bugs], [UI/UX Improvements], [Responsive Issues], [Feature-specific categories based on the test phases]

Execute now using the Playwright MCP tools. Do not stop until you have completed your assigned stream.  
```

**Output:** `tests/qa-audit-report.md`

## Step 6: Validate

- [ ] Every critical flow the user listed is covered by at least one test phase  
- [ ] Authentication setup matches the project's dev environment  
- [ ] Pre-flight checks cover all required services  
- [ ] Mobile responsive testing is included  
- [ ] No content from the source project (Nandonia) remains — verify the output references only the target project's technology