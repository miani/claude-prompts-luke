# Generate: Dependency Audit Command

Generate `.claude/commands/dependency-audit.md` — a command that audits all dependencies for vulnerabilities, outdated packages, and unused packages.

## Step 1: Read Project Context

Read **CLAUDE.md** for tech stack and project structure.

## Step 2: Extract Project Details

- **Package managers**: Detect which are used from the tech stack:  
  - Rust → Cargo (Cargo.toml, Cargo.lock)  
  - PHP → Composer (composer.json, composer.lock)  
  - Node.js → npm/yarn/pnpm (package.json, package-lock.json/yarn.lock/pnpm-lock.yaml)  
  - Python → pip/poetry (requirements.txt/pyproject.toml)  
  - Go → Go modules (go.mod, go.sum)  
  - Ruby → Bundler (Gemfile, Gemfile.lock)  
  - .NET → NuGet (*.csproj)  
- **Source directories per layer**: Where to search for imports  
- **Config file locations**: Where indirect dependencies are referenced (vite.config, tailwind.config, webpack.config, etc.)

## Step 3: Generate `.claude/commands/dependency-audit.md`

```yaml  
---  
allowed-tools: Read, Glob, Grep, LS, Task, Bash  
description: Audit all dependencies for vulnerabilities, outdated packages, and unused packages  
---  
```

Body structure:

**[UNIVERSAL] Audit Categories — include verbatim:**  
```  
## Audit Categories

### 1. Security Vulnerabilities  
Run automated audit tools to find known CVEs and security advisories.

### 2. Outdated Packages  
- **Major version behind**: Breaking changes, assess migration effort  
- **Minor version behind**: New features, usually safe to update  
- **Patch version behind**: Bug fixes, should update

### 3. Abandoned/Unmaintained Packages  
- Not updated in >2 years  
- Marked deprecated  
- Archived on GitHub  
- Known replacement recommended

### 4. Unused Dependencies  
Packages listed but never imported in source code.

### 5. Miscategorized Dependencies  
Production packages in devDependencies or vice versa.

### 6. License Compliance  
- AGPL/GPL (copyleft)  
- No license specified  
- Unusual or restrictive licenses  
```

**[TAILOR] Phase 1 — Automated audits.** Generate the correct audit commands for each detected package manager:

| Package Manager | Audit Command | Outdated Command |  
|---|---|---|  
| Cargo | `cd [dir] && cargo audit` | `cd [dir] && cargo outdated` |  
| Composer | `cd [dir] && composer audit` | `cd [dir] && composer outdated --direct` |  
| npm | `cd [dir] && npm audit` | `cd [dir] && npm outdated` |  
| yarn | `cd [dir] && yarn audit` | `cd [dir] && yarn outdated` |  
| pip | `cd [dir] && pip-audit` | `cd [dir] && pip list --outdated` |  
| Go | `cd [dir] && govulncheck ./...` | `cd [dir] && go list -m -u all` |

Add fallback: "If any tool is not installed, note it in the report and proceed."

**[TAILOR] Phase 2 — Unused dependency analysis.** Spawn one sub-task per package manager. Each sub-task:  
1. Reads the dependency file (package.json, composer.json, Cargo.toml, etc.)  
2. For each dependency, searches the source directory for imports/usage  
3. Accounts for indirect usage (config files, plugins, proc-macro crates, type packages)  
4. Writes findings to `/tmp/depaudit_[layer].md`

**Phase 3 — Abandoned package check.** For each dependency, check if the package is:  
- Not updated in >2 years  
- Marked deprecated by its registry  
- Archived on its source repository  
Write findings to `/tmp/depaudit_abandoned.md`.

**Phase 4 — Sequential report synthesis.** Read all `/tmp/depaudit_*.md` files sequentially and synthesize into a single report. Include: vulnerability table, outdated table, unused table, abandoned table, license table, and a recommended update plan with immediate/short-term/planned sections.

**Phase 5 — Cleanup.** Remove temp files:  
```bash  
rm -f /tmp/depaudit_*.md  
```

**[UNIVERSAL] Output format and report structure** — use the same table-based format from the reference command (vulnerability table, outdated table, unused table, license table, recommended update plan with immediate/short-term/planned sections).

**Output:** `docs/reports/dependency_audit_<DATE>.md`

## Step 4: Validate

- [ ] Every package manager in the project has corresponding audit commands  
- [ ] Source directories match where each language's code lives  
- [ ] Config file locations are included for indirect dependency checking