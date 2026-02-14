# Setup

## Install

```bash
pnpm add -D @biomejs/biome
```

## Baseline `biome.json`

```json
{
  "$schema": "https://biomejs.dev/schemas/2.3.13/schema.json",
  "vcs": {
    "enabled": true,
    "clientKind": "git",
    "useIgnoreFile": true
  },
  "files": {
    "ignoreUnknown": true
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 4,
    "lineEnding": "lf",
    "lineWidth": 120
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true
    }
  },
  "css": {
    "parser": {
      "tailwindDirectives": true
    }
  },
  "javascript": {
    "formatter": {
      "quoteStyle": "single",
      "jsxQuoteStyle": "double",
      "semicolons": "asNeeded",
      "trailingCommas": "es5"
    }
  }
}
```

Update schema version to match installed Biome:

```bash
pnpm biome --version
```

## `package.json` Scripts

```json
{
  "scripts": {
    "lint": "biome lint .",
    "lint:fix": "biome lint --write .",
    "format": "biome format .",
    "format:fix": "biome format --write .",
    "check": "biome check .",
    "check:fix": "biome check --write .",
    "ci": "biome ci ."
  }
}
```

Default recommendation:
- local: `pnpm check:fix`
- CI: `pnpm ci`

## VS Code Integration

Use `.vscode/settings.json`:

```json
{
  "[javascript]": { "editor.defaultFormatter": "biomejs.biome", "editor.formatOnSave": true },
  "[typescript]": { "editor.defaultFormatter": "biomejs.biome", "editor.formatOnSave": true },
  "[javascriptreact]": { "editor.defaultFormatter": "biomejs.biome", "editor.formatOnSave": true },
  "[typescriptreact]": { "editor.defaultFormatter": "biomejs.biome", "editor.formatOnSave": true },
  "[json]": { "editor.defaultFormatter": "biomejs.biome", "editor.formatOnSave": true },
  "[jsonc]": { "editor.defaultFormatter": "biomejs.biome", "editor.formatOnSave": true }
}
```

Disable conflicting workspace formatters (ESLint/Prettier) when migrating.

## Optional Pre-commit Hooks

Install and configure:

```bash
pnpm add -D husky lint-staged
pnpm exec husky init
```

`package.json` snippet:

```json
{
  "lint-staged": {
    "*.{js,ts,jsx,tsx,json,css}": ["biome check --write --no-errors-on-unmatched"]
  }
}
```

`.husky/pre-commit`:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

pnpm lint-staged
```
