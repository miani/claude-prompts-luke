# Generate: Tech Debt Audit Command

Generate `.claude/commands/tech-debt-audit.md` — an orchestrator command that scans all layers for tech debt using parallel sub-tasks.

**Prerequisite:** The tech-debt agent must exist at `.claude/agents/tech-debt.md`. Generate it first using the "Generate: Tech Debt Agent" prompt.

## Step 1: Read Project Context

Read **CLAUDE.md** for tech stack, project structure, shared constants.

## Step 2: Extract Project Details

- **Project layers and directories**  
- **Package managers per layer** (for unused dependency scanning)  
- **Debug/logging functions per language**  
- **Shared/generated files** (to understand cross-layer constants)

## Step 3: Generate `.claude/commands/tech-debt-audit.md`

```yaml  
---  
allowed-tools: Read, Glob, Grep, LS, Task, Bash  
description: Scan all repos for tech debt, dead code, TODOs, and abandoned artifacts  
---  
```

Body — same orchestrator pattern as performance-review-all:

1. **Opening**: Persona + objective referencing the tech-debt agent's scan categories  
2. **Reference documentation**: CLAUDE.md, architectural patterns  
3. **Phase 0**: Check past audits (`ls -la docs/reports/tech_debt_audit_*.md`)  
4. **Phase 1**: Parallel sub-task dispatch — one per layer plus one for root/cross-layer. Each sub-task:  
   - Uses the tech-debt agent's scan categories  
   - Includes verification rules: "Before claiming unused, search ALL repos"  
   - Writes to `/tmp/techdebt_[layer].md`  
   - Returns brief confirmation  
5. **Phase 2**: Verification pass (read temp files, verify findings, remove false positives)  
6. **Phase 3**: Sequential report synthesis (one section at a time)  
7. **Phase 4**: Cleanup (`rm -f /tmp/techdebt_*.md`)  
8. **Output**: `docs/reports/tech_debt_audit_<DATE>.md`

**[TAILOR]** Each sub-task prompt should reference:  
- The specific directories for that layer  
- The package manager for that layer (for dependency scanning)  
- The debug functions for that layer's language  
- "IMPORTANT: Verify every 'unused' finding by searching for references across the ENTIRE project"

**[TAILOR]** The root/cross-layer sub-task should check:  
- Shared constants/configs for unused values  
- Docker/infrastructure for stale services  
- Documentation for references to removed features  
- Environment files for stale/unused values

## Step 4: Validate

- [ ] One sub-task per layer plus root/cross-layer  
- [ ] Debug functions match each layer's language  
- [ ] Package managers match each layer  
- [ ] Verification rules in every sub-task prompt