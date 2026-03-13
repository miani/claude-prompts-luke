# Claude Prompts — Luke's Setup

A collection of Claude Code setup prompts from Luke Doherty's [Claude Code Setup Guidance](https://adahealth.atlassian.net/wiki/spaces/~5d7f75cf4f9cf00d60211b9b/pages/1057304487198738/Claude+Code+Setup+Guidance), section 3.3.

Each prompt, when run in Claude Code, generates a tailored agent or command for your specific project by reading your `CLAUDE.md` and any context files in `.claude/context/`.

---

## Structure

### `code-review/` — Code Review
Automated code review across the full codebase or just a branch's changes.

| File | Description |
|------|-------------|
| `generate-code-review-agent.md` | Generate the core code review agent (run first) |
| `generate-code-review-all-command.md` | Generate a `/code-review-all` command to review the entire codebase |
| `generate-code-review-branch-command.md` | Generate a `/code-review-branch` command to review only branch changes |

### `design-review/` — Design Review (Frontend)
UI/UX review using Playwright. Requires a frontend component.

| File | Description |
|------|-------------|
| `generate-design-review-agent.md` | Generate the core design review agent (run first) |
| `generate-design-review-all-command.md` | Generate a `/design-review-all` command to review the full frontend |
| `generate-design-review-branch-command.md` | Generate a `/design-review-branch` command to review branch UI changes |

### `security-review/` — Security Review
Security vulnerability scanning across the full codebase or branch changes.

| File | Description |
|------|-------------|
| `generate-security-review-agent.md` | Generate the core security review agent (run first) |
| `generate-security-review-all-command.md` | Generate a `/security-review-all` command for a full codebase scan |
| `generate-security-review-branch-command.md` | Generate a `/security-review-branch` command for branch-scoped scanning |

### `performance-review/` — Performance Review
Identifies bottlenecks across all layers — slow queries, bundle size, missing indexes, etc.

| File | Description |
|------|-------------|
| `generate-performance-review-agent.md` | Generate the core performance review agent (run first) |
| `generate-performance-review-command.md` | Generate a `/performance-review` orchestrator command |

### `tech-debt/` — Tech Debt Audit
Finds stale TODOs, dead code, unused dependencies, orphaned files, and inconsistent patterns.

| File | Description |
|------|-------------|
| `generate-tech-debt-agent.md` | Generate the core tech debt agent (run first) |
| `generate-tech-debt-audit-command.md` | Generate a `/tech-debt-audit` orchestrator command |

### `e2e-testing/` — End-to-End Testing (Frontend)
Automated QA of critical user flows using Playwright. Requires a frontend component.

| File | Description |
|------|-------------|
| `generate-e2e-tester-agent.md` | Generate the core E2E tester agent (run first) |
| `generate-e2e-test-command.md` | Generate a `/app-e2e-test` command (interactive — answers questions about flows) |

### `dependency-audit/` — Dependency Audit
Audits all third-party packages for vulnerabilities, outdated versions, and license issues.

| File | Description |
|------|-------------|
| `generate-dependency-audit-command.md` | Generate a `/dependency-audit` command |

### `testing/` — Test Creation & Execution
Find and fill test coverage gaps, and run the full test suite across all layers.

| File | Description |
|------|-------------|
| `generate-test-creation-command.md` | Generate a `/test-creation` command to find and fill test coverage gaps |
| `generate-test-execution-command.md` | Generate a `/test-execution` command to run all tests with unified reporting |

### `production/` — Production Readiness
Pre-launch audit for hardcoded dev URLs, exposed debug ports, missing headers, and more.

| File | Description |
|------|-------------|
| `generate-production-readiness-command.md` | Generate a `/production-readiness` command |

---

## How to use

1. Ensure your `CLAUDE.md` is well set up with your project's tech stack, structure, and conventions.
2. Open a Claude Code session in your project root.
3. Copy and paste the prompt content into Claude Code.
4. For agents (e.g. `generate-code-review-agent.md`), always run these **before** their corresponding commands.
5. Reload VSCode extensions after generating each command to enable it.
