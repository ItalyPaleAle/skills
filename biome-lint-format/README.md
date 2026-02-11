# Biome Linting and Formatting Skill

This skill provides comprehensive guidance for setting up [Biome](https://biomejs.dev) - a fast, all-in-one toolchain for linting and formatting JavaScript, TypeScript, JSX, TSX, JSON, and CSS.

Biome is designed as a drop-in replacement for ESLint and Prettier, with significantly faster performance (10-100x faster) and zero configuration needed to get started.

## What This Skill Covers

1. Biome installation and setup
2. Complete configuration with best practices
3. VS Code integration with format on save
4. Pre-commit hooks with Husky and lint-staged
5. Migration guide from ESLint + Prettier
6. Package.json scripts for linting and formatting
7. Tailwind CSS directive support
8. Rule customization examples
9. Comprehensive troubleshooting guide

## Installation

### Using Claude Code CLI

```bash
npx skills add https://github.com/ItalyPaleAle/skills/tree/main/biome-lint-format
```

### Using Claude Projects (claude.ai)

Add the following URL to your Project knowledge:

```
https://raw.githubusercontent.com/ItalyPaleAle/skills/main/biome-lint-format/SKILL.md
```

### Manual Installation

Download [SKILL.md](./SKILL.md) and add it to your Claude Code project or Claude Project knowledge base.

## Usage

Once installed, you can invoke this skill by asking Claude to:

- "Set up Biome for linting and formatting"
- "Configure Biome"
- "Add Biome to my project"
- "Replace ESLint and Prettier with Biome"
- "Set up Biome with Tailwind CSS support"

Claude will use this skill to set up Biome with the recommended configuration and integrate it with your development workflow.

## Features

### Why Biome?

- **10-100x faster** than ESLint + Prettier
- **Single tool** for both linting and formatting
- **Zero config** - works out of the box
- **Better error messages** with suggested fixes
- **First-class TypeScript support** - no extra packages needed
- **Native VCS integration** - respects `.gitignore`
- **Official VS Code extension** with fast feedback

### What's Included

- Production-ready `biome.json` configuration
- Package.json scripts for `lint`, `format`, `check`, and `ci`
- VS Code settings for format on save and auto-fix
- Pre-commit hooks setup with Husky
- Tailwind CSS directive support
- Migration guide from ESLint + Prettier
- Rule customization examples

### Configuration Highlights

The included configuration provides:

- 4-space indentation
- Single quotes for JavaScript, double quotes for JSX
- Semicolons only when needed
- 120 character line width
- Warns on unused variables and imports
- Supports Tailwind CSS directives
- Git integration enabled

## When to Use Biome

✅ **Use Biome when:**

- Starting a new JavaScript/TypeScript project
- You want faster linting and formatting
- You're tired of ESLint/Prettier config conflicts
- You want a single tool for both linting and formatting

⚠️ **Consider keeping ESLint when:**

- You rely on ESLint plugins not available in Biome (e.g., `eslint-plugin-react-hooks`)
- You have custom organization-specific ESLint rules
- Migration cost is too high for existing projects

## Requirements

- Node.js 22+
- A JavaScript or TypeScript project
- pnpm, npm, or yarn package manager

## Quick Start

After adding the skill, ask Claude:

> "Set up Biome for my TypeScript project"

Claude will:

1. Install Biome as a dev dependency
2. Create a `biome.json` configuration
3. Add package.json scripts
4. Set up VS Code integration
5. Optionally configure pre-commit hooks

Then run:

```bash
# Check and fix all files
pnpm check:fix

# Or just check without fixing
pnpm check
```

## License

[MIT](../LICENSE.md)
