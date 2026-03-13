# Generate: Test Creation Command

Generate `.claude/commands/test-creation.md` — a command that analyzes the codebase, identifies missing tests, and creates test scaffolding.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — tech stack, development commands, project structure  
2. **.claude/context/testing.md** (if exists) — testing standards, test matrix, critical paths  
3. **.claude/context/architectural_patterns.md** (if exists) — patterns that need test coverage

## Step 2: Extract Project Details

- **Test frameworks per layer**: Detect from tech stack (e.g., Vitest for React, PHPUnit for Symfony, pytest for Python, Jest for Node.js, Cargo test for Rust)  
- **Test directories per layer**: Where tests live (e.g., `__tests__/`, `tests/`, `spec/`)  
- **Test commands**: How to run tests per layer (from CLAUDE.md Development Commands)  
- **Critical paths**: Key user flows that must have test coverage (from testing.md or domain context)  
- **Project layers and source directories**

## Step 3: Generate `.claude/commands/test-creation.md`

```yaml  
---  
allowed-tools: Read, Write, Glob, Grep, LS, Bash, Task  
description: Analyze codebase and create missing test cases across all layers  
---  
```

Body — 5-phase structure:

**Phase 1: Codebase Analysis**  
- [TAILOR] For each layer, list what to scan:  
  - Test directory path  
  - What to look for (which modules have tests vs untested)  
  - Failure path test presence

**Phase 2: Test Gap Analysis**  
- Generate a test matrix report template per layer  
- [TAILOR] Table columns should match the layer's test types (unit, integration, API, component, E2E)

**Phase 3: Test Creation**  
- [TAILOR] For each layer, provide test scaffolding templates in the project's actual test framework:  
  - Success path test template  
  - Failure path test template  
  - Use the project's actual testing patterns and conventions

**Phase 4: Execution**  
- Spawn sub-tasks per layer for parallel test creation  
- [TAILOR] Include the actual test run commands from CLAUDE.md

**Phase 5: Reporting**  
- Test creation report template at `tests/test-creation-report.md`

**Execution Notes:**  
```  
- Do not skip failure path tests  
- Follow existing test patterns in each layer  
- Use proper mocking — match the project's existing mock approach  
- Verify with actual runs — don't just create files  
```

## Step 4: Validate

- [ ] Test frameworks match each layer's actual technology  
- [ ] Test directories match where tests actually live  
- [ ] Test run commands match CLAUDE.md  
- [ ] Test scaffolding templates use the correct assertion library and patterns