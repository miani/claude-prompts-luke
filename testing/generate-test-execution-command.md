# Generate: Test Execution Command

Generate `.claude/commands/test-execution.md` — a command that runs the full test suite across all layers and generates a report.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — development commands, service URLs, container names  
2. **.claude/context/testing.md** (if exists) — test commands, file locations

## Step 2: Extract Project Details

- **Test commands per layer**: Exact commands to run tests (from CLAUDE.md)  
- **Service dependencies**: Which services must be running for tests (databases, APIs, etc.)  
- **Pre-flight health checks**: How to verify services are up  
- **Container names**: If using Docker, which containers run which tests  
- **Test frameworks**: To parse output correctly (Mocha, PHPUnit, Vitest, pytest, etc.)

## Step 3: Generate `.claude/commands/test-execution.md`

```yaml  
---  
allowed-tools: Bash, Read, Write, Glob, LS, Task  
description: Execute all tests across all layers with comprehensive reporting  
---  
```

Body structure:

**[TAILOR] Pre-Flight Checks:** Generate health check commands for each required service:  
```bash  
# Verify [service name] is running  
[health check command]  
```

**[TAILOR] Per-Layer Test Execution:** For each layer, generate:  
- The exact test command (from CLAUDE.md)  
- How to run specific test suites (unit, integration, etc.)  
- What output to parse for (framework-specific)

**[TAILOR] Parallel Execution:** Show how to run all layers in parallel.

**[TAILOR] Failure Handling:** For each layer, provide troubleshooting steps:  
- How to check if dependencies are running  
- Common failure causes and fixes  
- How to clear caches, reinstall dependencies, etc.

**[UNIVERSAL] Report Template:**  
```markdown  
# Test Execution Report

**Generated**: [timestamp]  
**Commit**: [git commit hash]  
**Branch**: [git branch]

## Summary

| Layer | Passed | Failed | Skipped | Duration |  
|-------|--------|--------|---------|----------|  
| [Layer 1] | X | Y | Z | Xs |  
| **Total** | **X** | **Y** | **Z** | **Xs** |

## Overall Status: [PASS/FAIL]

[Per-layer details with failed test output]

## Recommendations  
### Critical Failures (Blocking)  
### Warnings  
### Next Steps  
```

**[TAILOR] Quick Commands Reference:** A single code block with all test commands for easy copy-paste.

**Output:** Generate report at `tests/test-execution-report.md`

## Step 4: Validate

- [ ] Pre-flight checks cover all required services  
- [ ] Test commands match CLAUDE.md exactly  
- [ ] Failure handling references correct container/service names  
- [ ] Quick commands reference is complete