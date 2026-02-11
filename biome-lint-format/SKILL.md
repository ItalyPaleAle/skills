---
name: biome-lint-format
version: 1.0.0
description: Set up Biome for fast linting and formatting in JavaScript/TypeScript projects
category: tooling
tags:
  - biome
  - linting
  - formatting
  - typescript
  - javascript
invocation:
  - "Set up Biome for linting and formatting"
  - "Configure Biome"
  - "Add Biome to my project"
  - "Replace ESLint and Prettier with Biome"
examples:
  - "Set up Biome with Tailwind CSS support"
  - "Configure Biome for a TypeScript project"
  - "Migrate from ESLint and Prettier to Biome"
---

# Skill: Configuring Linting and Formatting with Biome

## Overview

[Biome](https://biomejs.dev) is a fast, all-in-one toolchain for linting and formatting JavaScript, TypeScript, JSX, TSX, JSON, and CSS. It's designed as a drop-in replacement for ESLint and Prettier, with significantly faster performance (10-100x faster) and zero configuration needed to get started.

## When to Use This Skill

Use this skill when you need to:

- Set up linting and formatting for a JavaScript/TypeScript project
- Replace ESLint and Prettier with a faster, unified tool
- Enforce code quality and consistent formatting in your codebase
- Add pre-commit hooks to ensure code quality before commits
- Configure linting for projects using Tailwind CSS

## Prerequisites

- Node.js 22+ installed
- A JavaScript or TypeScript project
- pnpm, npm, or yarn package manager

---

## Why Biome?

### Advantages over ESLint + Prettier

- **Blazingly fast**: 10-100x faster than ESLint + Prettier
- **Single tool**: One tool for both linting and formatting (no configuration conflicts)
- **Zero config**: Works out of the box with sensible defaults
- **Better error messages**: Clear, actionable error messages with suggested fixes
- **First-class TypeScript support**: No need for `@typescript-eslint/*` packages
- **Built-in formatter**: No need to configure Prettier separately
- **Native VCS integration**: Automatically uses `.gitignore` to skip files
- **Editor integration**: Official VS Code extension with fast feedback

### When to Stick with ESLint + Prettier

- You rely on ESLint plugins that don't have Biome equivalents (e.g., `eslint-plugin-react-hooks`, `eslint-plugin-jsx-a11y`)
- You need custom ESLint rules specific to your organization
- Your team is heavily invested in ESLint/Prettier configs and migration cost is too high

---

## 1. Installation

```bash
pnpm add -D @biomejs/biome
```

Or with npm:

```bash
npm install --save-dev @biomejs/biome
```

---

## 2. Configuration

Create `biome.json` in your project root:

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
      "recommended": true,
      "correctness": {
        "noUnusedVariables": "warn",
        "noUnusedImports": "warn"
      },
      "style": {
        "noNonNullAssertion": "off",
        "useBlockStatements": "error"
      },
      "suspicious": {
        "noExplicitAny": "warn",
        "noDoubleEquals": "off"
      }
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

**Important:** Update the `$schema` version to match your installed Biome version. Check your version with:

```bash
pnpm biome --version
```

Then update the schema URL: `https://biomejs.dev/schemas/{VERSION}/schema.json`

---

## 3. Configuration Breakdown

### VCS Integration

```json
"vcs": {
  "enabled": true,
  "clientKind": "git",
  "useIgnoreFile": true
}
```

- Enables Git integration
- Automatically respects `.gitignore` patterns
- Biome will skip files that Git ignores (e.g., `node_modules/`, `dist/`)

### Files

```json
"files": {
  "ignoreUnknown": true
}
```

- Ignores files with unknown extensions
- Prevents errors when running Biome on mixed-content directories

### Formatter

```json
"formatter": {
  "enabled": true,
  "indentStyle": "space",
  "indentWidth": 4,
  "lineEnding": "lf",
  "lineWidth": 120
}
```

- **`indentStyle`**: `"space"` or `"tab"`
- **`indentWidth`**: Number of spaces per indent (common: 2 or 4)
- **`lineEnding`**: `"lf"` (Unix) or `"crlf"` (Windows)
- **`lineWidth`**: Max characters per line (common: 80, 100, or 120)

### Linter

```json
"linter": {
  "enabled": true,
  "rules": {
    "recommended": true,
    "correctness": {
      "noUnusedVariables": "warn",
      "noUnusedImports": "warn"
    },
    "style": {
      "noNonNullAssertion": "off",
      "useBlockStatements": "error"
    },
    "suspicious": {
      "noExplicitAny": "warn",
      "noDoubleEquals": "off"
    }
  }
}
```

- **`recommended: true`**: Enables all recommended rules
- **Rule severity levels**: `"off"`, `"warn"`, `"error"`

**Common rule overrides:**

- `noUnusedVariables`: Warn on unused variables (useful during development)
- `noUnusedImports`: Warn on unused imports
- `noNonNullAssertion`: Disabled to allow `!` non-null assertions in TypeScript
- `useBlockStatements`: Require curly braces for all control flow statements
- `noExplicitAny`: Warn on explicit `any` types
- `noDoubleEquals`: Disabled to allow `==` (if you prefer `===`, set to `"error"`)

### CSS (Tailwind Support)

```json
"css": {
  "parser": {
    "tailwindDirectives": true
  }
}
```

- Enables support for Tailwind CSS directives (`@tailwind`, `@apply`, `@layer`)
- Required if using Tailwind CSS v3 or v4

### JavaScript Formatter

```json
"javascript": {
  "formatter": {
    "quoteStyle": "single",
    "jsxQuoteStyle": "double",
    "semicolons": "asNeeded",
    "trailingCommas": "es5"
  }
}
```

- **`quoteStyle`**: `"single"` or `"double"` for JavaScript strings
- **`jsxQuoteStyle`**: `"single"` or `"double"` for JSX attributes
- **`semicolons`**: `"always"` or `"asNeeded"` (omits unnecessary semicolons)
- **`trailingCommas`**: `"none"`, `"es5"`, or `"all"`

---

## 4. Package.json Scripts

Add these scripts to your `package.json`:

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

### Script Descriptions

- **`lint`**: Check for linting errors (doesn't modify files)
- **`lint:fix`**: Fix auto-fixable linting errors
- **`format`**: Check for formatting issues (doesn't modify files)
- **`format:fix`**: Format all files
- **`check`**: Run both linting and formatting checks (recommended for most cases)
- **`check:fix`**: Fix both linting and formatting issues
- **`ci`**: Run checks in CI mode (fails on errors, no fixes)

### Recommended Workflow

For local development, use:

```bash
pnpm check:fix
```

This will fix both linting and formatting issues in one command.

For CI/CD pipelines, use:

```bash
pnpm ci
```

This will fail the build if any issues are found.

---

## 5. VS Code Integration

### Install the Biome Extension

1. Open VS Code
2. Go to Extensions (Cmd+Shift+X / Ctrl+Shift+X)
3. Search for "Biome"
4. Install the official extension: **Biome** by `biomejs`

### VS Code Settings

Add to `.vscode/settings.json`:

```json
{
  "[javascript]": {
    "editor.defaultFormatter": "biomejs.biome",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "quickfix.biome": "explicit",
      "source.organizeImports.biome": "explicit"
    }
  },
  "[typescript]": {
    "editor.defaultFormatter": "biomejs.biome",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "quickfix.biome": "explicit",
      "source.organizeImports.biome": "explicit"
    }
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "biomejs.biome",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "quickfix.biome": "explicit",
      "source.organizeImports.biome": "explicit"
    }
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "biomejs.biome",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "quickfix.biome": "explicit",
      "source.organizeImports.biome": "explicit"
    }
  },
  "[json]": {
    "editor.defaultFormatter": "biomejs.biome",
    "editor.formatOnSave": true
  },
  "[jsonc]": {
    "editor.defaultFormatter": "biomejs.biome",
    "editor.formatOnSave": true
  }
}
```

This configuration:

- Sets Biome as the default formatter for JS/TS/JSX/TSX/JSON files
- Enables format on save
- Automatically applies quick fixes on save
- Automatically organizes imports on save

### Disable Conflicting Extensions

If you have ESLint or Prettier extensions installed, disable them for your workspace to avoid conflicts:

1. Click on the extension
2. Click "Disable (Workspace)"

Or add to `.vscode/extensions.json`:

```json
{
  "recommendations": ["biomejs.biome"],
  "unwantedRecommendations": ["dbaeumer.vscode-eslint", "esbenp.prettier-vscode"]
}
```

---

## 6. Pre-commit Hooks (Optional)

Use [Husky](https://typicode.github.io/husky/) and [lint-staged](https://github.com/lint-staged/lint-staged) to run Biome on staged files before commits.

### Installation

```bash
pnpm add -D husky lint-staged
```

### Setup

Initialize Husky:

```bash
pnpm exec husky init
```

### Configure lint-staged

Add to `package.json`:

```json
{
  "lint-staged": {
    "*.{js,ts,jsx,tsx,json,css}": ["biome check --write --no-errors-on-unmatched"]
  }
}
```

### Create pre-commit hook

Edit `.husky/pre-commit`:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

pnpm lint-staged
```

Make it executable:

```bash
chmod +x .husky/pre-commit
```

Now Biome will automatically check and fix staged files before every commit.

---

## 7. Usage Examples

### Format a single file

```bash
pnpm biome format --write src/App.tsx
```

### Lint a specific directory

```bash
pnpm biome lint src/
```

### Check and fix all issues

```bash
pnpm biome check --write .
```

### Check with error summary

```bash
pnpm biome check --error-on-warnings .
```

### Organize imports only

```bash
pnpm biome check --write --organize-imports-enabled=true --formatter-enabled=false --linter-enabled=false .
```

---

## 8. Ignoring Files

Biome respects `.gitignore` by default when VCS integration is enabled. For additional ignore patterns, create a `biome.json` ignore list:

```json
{
  "files": {
    "ignore": [
      "dist/**",
      "build/**",
      "coverage/**",
      "*.min.js",
      "public/vendor/**"
    ]
  }
}
```

Or use inline comments:

```js
// biome-ignore lint/suspicious/noExplicitAny: Legacy code, will refactor later
const data: any = getUntypedData();
```

---

## 9. Migration from ESLint + Prettier

### Step 1: Remove old dependencies

```bash
pnpm remove eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-config-prettier eslint-plugin-prettier
```

Remove related config files:

```bash
rm .eslintrc.json .eslintrc.js .prettierrc .prettierrc.json .prettierignore
```

### Step 2: Install Biome

```bash
pnpm add -D @biomejs/biome
```

### Step 3: Initialize Biome config

```bash
pnpm biome init
```

This creates a default `biome.json`. Replace it with the configuration from section 2.

### Step 4: Update package.json scripts

Replace ESLint/Prettier scripts with Biome scripts (see section 4).

### Step 5: Update VS Code settings

Update `.vscode/settings.json` to use Biome (see section 5).

### Step 6: Run initial check

```bash
pnpm check:fix
```

This will format all files and fix auto-fixable linting issues. Review the changes carefully.

### Step 7: Update CI/CD pipelines

Replace ESLint/Prettier commands in your CI config with:

```bash
pnpm biome ci .
```

### Known Limitations

Biome doesn't support all ESLint plugins. If you need these, you may need to keep ESLint alongside Biome:

- `eslint-plugin-react-hooks`
- `eslint-plugin-jsx-a11y`
- `eslint-plugin-import`
- Custom organization-specific ESLint rules

In this case, you can run both:

```json
{
  "scripts": {
    "lint": "biome check . && eslint .",
    "lint:fix": "biome check --write . && eslint --fix ."
  }
}
```

---

## 10. Troubleshooting

### Biome not finding config file

**Problem:** Biome ignores `biome.json` and uses default config.

**Solution:**

- Ensure `biome.json` is in the project root (same directory as `package.json`)
- Check for syntax errors in `biome.json` with `pnpm biome check biome.json`
- Restart VS Code or reload window (Cmd+Shift+P → "Developer: Reload Window")

---

### Format on save not working in VS Code

**Problem:** VS Code not formatting files on save.

**Solution:**

1. Check that Biome extension is installed and enabled
2. Verify `.vscode/settings.json` has correct configuration (see section 5)
3. Check if another formatter is conflicting:
   - Disable ESLint and Prettier extensions for workspace
   - Check user settings for conflicting formatter settings
4. Restart VS Code

---

### Biome formatting conflicts with Prettier

**Problem:** Some files are formatted by Prettier, some by Biome.

**Solution:**

- Remove Prettier from `package.json` dependencies
- Delete `.prettierrc` and `.prettierignore`
- Disable Prettier VS Code extension for workspace
- Run `pnpm biome format --write .` to reformat all files with Biome

---

### TypeScript errors not showing in VS Code

**Problem:** Biome shows linting errors but not TypeScript type errors.

**Solution:**

Biome is a linter, not a type checker. It doesn't replace `tsc`. For type checking:

1. Keep TypeScript installed
2. Use `tsc --noEmit` for type checking
3. Add to `package.json`:

   ```json
   {
     "scripts": {
       "typecheck": "tsc --noEmit",
       "validate": "pnpm typecheck && pnpm check"
     }
   }
   ```

4. VS Code's TypeScript language service handles type checking separately

---

### Biome slow on large projects

**Problem:** Biome takes a long time to check files.

**Solutions:**

1. **Use VCS integration** to skip ignored files:

   ```json
   {
     "vcs": {
       "enabled": true,
       "useIgnoreFile": true
     }
   }
   ```

2. **Exclude large directories:**

   ```json
   {
     "files": {
       "ignore": ["dist/**", "build/**", "coverage/**"]
     }
   }
   ```

3. **Run Biome only on staged files** with lint-staged (see section 6)

---

### Schema validation errors

**Problem:** VS Code shows errors in `biome.json` about unknown properties.

**Solution:**

Update the `$schema` URL to match your Biome version:

```bash
# Check version
pnpm biome --version
# Output: Version: 2.3.13
```

Then update `biome.json`:

```json
{
  "$schema": "https://biomejs.dev/schemas/2.3.13/schema.json"
}
```

---

## 11. Rule Customization Examples

### Enforce `===` over `==`

```json
{
  "linter": {
    "rules": {
      "suspicious": {
        "noDoubleEquals": "error"
      }
    }
  }
}
```

### Allow `console.log` in development

```json
{
  "linter": {
    "rules": {
      "suspicious": {
        "noConsoleLog": "off"
      }
    }
  }
}
```

### Require explicit function return types

```json
{
  "linter": {
    "rules": {
      "style": {
        "useExplicitType": "error"
      }
    }
  }
}
```

### Enforce consistent naming conventions

```json
{
  "linter": {
    "rules": {
      "style": {
        "useNamingConvention": {
          "level": "error",
          "options": {
            "strictCase": false
          }
        }
      }
    }
  }
}
```

---

## Additional Resources

- [Biome Official Documentation](https://biomejs.dev)
- [Biome Rules Reference](https://biomejs.dev/linter/rules/)
- [Biome VS Code Extension](https://marketplace.visualstudio.com/items?itemName=biomejs.biome)
- [Migration Guide from ESLint](https://biomejs.dev/guides/migrate-eslint-prettier/)
- [Biome GitHub Repository](https://github.com/biomejs/biome)

---

## Checklist

- [ ] Biome installed as dev dependency
- [ ] `biome.json` created with proper schema version
- [ ] Scripts added to `package.json` for `check`, `lint`, and `format`
- [ ] VS Code extension installed
- [ ] `.vscode/settings.json` configured for format on save
- [ ] ESLint and Prettier removed (if migrating)
- [ ] Pre-commit hooks set up (optional but recommended)
- [ ] CI/CD pipeline updated to use `biome ci`
- [ ] Team informed about new linting/formatting tool
