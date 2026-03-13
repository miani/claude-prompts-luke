# Generate: Production Readiness Command

Generate `.claude/commands/production-readiness.md` — a comprehensive pre-production audit tailored to this project's stack.

## Step 1: Read Project Context

Read these files:

1. **CLAUDE.md** (required) — tech stack, service URLs, environment variables, container names  
2. **.claude/context/architectural_patterns.md** (if exists)  
3. **All other .claude/context/ files** — to understand the full application

## Step 2: Extract Project Details

- **Tech stack**: Every technology, to determine which audit categories apply  
- **Environment variables**: What needs production values  
- **Service URLs**: What needs to point to production  
- **Container/infrastructure setup**: Docker, Kubernetes, serverless, etc.  
- **Frontend build tool**: For bundle optimization checks  
- **Web server**: Nginx, Apache, Caddy, etc.  
- **Database**: Type and config location  
- **Authentication**: Auth mechanism to verify in production  
- **Any dev-mode backdoors**: Dev toolbars, mock auth, test accounts that must be disabled

## Step 3: Generate `.claude/commands/production-readiness.md`

```yaml  
---  
allowed-tools: Read, Glob, Grep, LS, Task, Bash  
description: Comprehensive pre-production audit covering configuration, security hardening, infrastructure, and bundle optimization  
---  
```

Body structure:

**[TAILOR] Opening with project context:**  
```  
You are a production readiness engineer conducting a comprehensive pre-launch audit.

OBJECTIVE:  
Systematically verify that all layers are production-ready: secure configuration, hardened infrastructure, optimized delivery, proper error handling, and no development artifacts leaking to users.

IMPORTANT CONTEXT:  
- [Brief description of what the application is]  
- [Key production concerns specific to this project, e.g., "The app uses a devToolbar for testing that MUST be disabled in production"]  
```

**[TAILOR] Audit Categories — generate only the categories relevant to the project's tech stack.** Choose from these templates:

**Category: Environment & Configuration** (always include)  
- For each environment variable in CLAUDE.md, generate a production check  
- Check for hardcoded localhost/dev URLs in source code  
- Verify .env files are in .gitignore  
- Check framework-specific production mode settings (APP_ENV=prod, NODE_ENV=production, etc.)

**Category: Security Hardening** (always include)  
- HTTP security headers (HSTS, CSP, X-Frame-Options, etc.)  
- CORS configuration  
- Error exposure (stack traces, debug info)  
- Dev-mode backdoors

**Category: Web Server** (if Nginx/Apache/Caddy detected)  
- TLS configuration  
- Compression (gzip/brotli)  
- Static asset caching headers  
- Performance tuning  
- Dotfile blocking

**Category: Container/Infrastructure** (if Docker/K8s detected)  
- No exposed debug ports  
- Resource limits  
- Health checks  
- Restart policies  
- Log rotation

**Category: Backend Runtime** (if backend detected)  
- PHP: PHP-FPM config, OPcache, expose_php  
- Python: Gunicorn/uWSGI workers, debug mode  
- Node.js: PM2 config, cluster mode  
- Database connection pooling and tuning

**Category: Frontend Bundle & Assets** (if frontend detected)  
- Bundle size analysis  
- Code splitting verification  
- Source maps disabled in production  
- Image optimization  
- Font loading

**Category: Monitoring & Observability** (always include)  
- Health check endpoints  
- Logging configuration  
- Error tracking integration

**Category: Domain-Specific** (if applicable)  
- Blockchain: Program authority, priority fees, RPC provider, wallet support  
- Payments: Webhook verification, idempotency, PCI compliance  
- Auth: Session management, token rotation, password policies

**[UNIVERSAL] Methodology — parallel sub-task pattern:**

**Phase 0: Check past audits.**  
```bash  
ls -la docs/reports/production_readiness_*.md 2>/dev/null  
```  
If past audits exist, read the most recent one and note what was previously flagged. Verify which items have been addressed.

**Phase 1: Parallel sub-tasks.** Dispatch one sub-task per audit category group (Configuration & Security, Infrastructure, Bundle & Assets, Monitoring & Domain-Specific). Each sub-task writes findings to `/tmp/prodready_[category].md`.

**Phase 2: Verification pass.** Before including any finding in the final report, verify it by searching the codebase. Do not report assumed issues — only confirmed ones.

**Phase 3: Sequential report synthesis.** Read all `/tmp/prodready_*.md` files sequentially and synthesize into a single report using the 3-bucket structure below.

**Phase 4: Cleanup.**  
```bash  
rm -f /tmp/prodready_*.md  
```

**[UNIVERSAL] Output format — 3-bucket structure:**  
```  
## Bucket 1: Implement Now (dev environment, benefits all environments)  
Code changes, asset optimizations, and build fixes that improve quality regardless of environment.

## Bucket 2: Needed for Staging AND Production  
Infrastructure, deployment configuration, and security hardening for any publicly-accessible environment.

## Bucket 3: Production Only  
Items specific to production's unique characteristics (real money, key management, compliance).  
```

Include a remediation checklist with specific action items per bucket.

**Output:** `docs/reports/production_readiness_<DATE>.md`

## Step 4: Validate

- [ ] Only categories relevant to the project's tech stack are included  
- [ ] Every environment variable from CLAUDE.md has a production check  
- [ ] Dev-mode backdoors are identified and checked  
- [ ] 3-bucket output structure is preserved