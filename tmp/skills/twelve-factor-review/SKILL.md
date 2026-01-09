---
name: twelve-factor-review
description: Review PHP/Laravel code for Twelve-Factor App compliance. Use when reviewing code architecture, configuration management, deployment readiness, or checking best practices for cloud-native applications. Trigger on requests like "12-Factor review", "check Twelve-Factor compliance", "review for cloud-native best practices", or when examining Laravel config, .env files, service connections, logging, or process management.
---

# Twelve-Factor App Review for Laravel

Review Laravel/PHP code against the Twelve-Factor App methodology for building cloud-native, scalable applications.

## Quick Start

1. Identify target files/directories to review
2. Check each applicable factor from the 12 principles
3. Report findings with severity levels and actionable fixes

## Review Workflow

### Step 1: Gather Context

Read the target files and identify which factors apply:
- Config files: `.env`, `config/*.php` → Factors III, IV, X
- Dependencies: `composer.json`, `composer.lock` → Factor II
- Application code → Factors VI, XI, XII
- Docker/deployment files → Factors V, VII, VIII, IX

### Step 2: Apply Checklist

For each applicable factor, check the criteria in [references/laravel-checklist.md](references/laravel-checklist.md).

### Step 3: Report Findings

Format findings as:

```
## Factor [Number]: [Name]
**Status**: ✅ Compliant | ⚠️ Partial | ❌ Non-compliant

**Findings**:
- [Specific issue or compliance note]

**Recommendation** (if needed):
- [Actionable fix with code example]
```

## The Twelve Factors (Summary)

| # | Factor | Laravel Focus |
|---|--------|---------------|
| I | Codebase | Single repo, Git-managed |
| II | Dependencies | composer.json/lock, no global deps |
| III | Config | .env usage, no hardcoded values |
| IV | Backing Services | Database/cache/queue as env config |
| V | Build, Release, Run | Separate build artifacts |
| VI | Processes | Stateless, session in Redis/DB |
| VII | Port Binding | Self-contained via artisan serve |
| VIII | Concurrency | Queue workers, horizontal scaling |
| IX | Disposability | Fast boot, graceful shutdown |
| X | Dev/Prod Parity | Same services across environments |
| XI | Logs | Log to stdout, use Log facade |
| XII | Admin Processes | Artisan commands, migrations |

## Detailed Checklist

See [references/laravel-checklist.md](references/laravel-checklist.md) for comprehensive Laravel-specific checks per factor.
