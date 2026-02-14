---
name: biome-lint-format
description: Set up Biome for fast linting and formatting in JavaScript/TypeScript projects, including editor integration, package scripts, optional pre-commit hooks, and migration from ESLint + Prettier. Use when adding or standardizing lint/format tooling, replacing ESLint/Prettier, or troubleshooting Biome configuration and workflow issues.
---

# Biome Lint Format

Set up Biome with a consistent workflow and minimal friction.

## Workflow

1. Confirm project context:
- Package manager
- JavaScript/TypeScript scope
- Keep ESLint/Prettier or migrate fully
- Need Tailwind CSS directive support
- Need pre-commit hooks

2. Install Biome and create baseline configuration.

3. Add project scripts for lint/format/check/ci flows.

4. Configure editor integration (VS Code) if requested.

5. Add optional pre-commit checks with Husky + lint-staged if requested.

6. If migrating from ESLint + Prettier, remove old config carefully and run a first auto-fix pass.

7. Validate setup by running check commands and confirming CI command behavior.

## Reference Map

Load only the reference needed for the current request:

- `references/setup.md`
Use for installation, baseline `biome.json`, scripts, VS Code setup, pre-commit setup, and command examples.

- `references/migration.md`
Use only when migrating from ESLint + Prettier or running a hybrid temporary setup.

- `references/troubleshooting.md`
Use for config detection issues, editor conflicts, schema/version mismatches, and performance tuning.

- `references/rule-customization.md`
Use only when the user asks for concrete rule customizations.

## Guardrails

- Keep `SKILL.md` short and procedural; store details in references.
- Use `biome check` as the default all-in-one command.
- Do not claim Biome replaces `tsc` type checking.
