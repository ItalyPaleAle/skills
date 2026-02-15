# Go Package Skill Creator Skill

This skill generates skills for Go packages by fetching documentation from pkg.go.dev and creating structured, usage-focused guides with:

- **Automated documentation fetching**
- **Context-aware complexity analysis**
- **Runnable code examples**
- **Progressive disclosure structure**
- **Best practices and error handling**

## What This Skill Covers

1. Package information gathering (import path, examples, focus areas)
2. Documentation fetching from pkg.go.dev
3. Package complexity analysis (simple, medium, complex)
4. Skill identity derivation (naming and description)
5. Skill structure creation (SKILL.md + references/)
6. Template-based skill generation with runnable examples
7. Validation and quality checks

## Installation

### Using Claude Code CLI

```bash
npx skills add https://github.com/ItalyPaleAle/skills/tree/main/go-package-skill-creator
```

### Using Claude Projects (claude.ai)

Add the following URL to your Project knowledge:

```
https://raw.githubusercontent.com/ItalyPaleAle/skills/main/go-package-skill-creator/SKILL.md
```

### Manual Installation

Download [SKILL.md](./SKILL.md) and add it to your Claude Code project or Claude Project knowledge base.

## Usage

Once installed, you can invoke this skill by asking Claude to:

- "Create a skill for github.com/golang-jwt/jwt/v5"
- "Generate a skill for the lestrrat-go/jwx package"
- "Make a skill to help use github.com/go-chi/chi"
- "Build a skill for Go package X"

Claude will gather the package information, fetch documentation, analyze complexity, and generate a complete skill with usage patterns, examples, and best practices.

## Features

### Intelligent Documentation

- Fetches package documentation from pkg.go.dev
- Extracts official examples and code snippets
- Identifies main types, functions, and patterns
- Analyzes subpackages and dependencies

### Context-Aware Structure

- **Simple packages**: Single SKILL.md (200-400 lines)
- **Medium packages**: SKILL.md with optional references/ (400-600 lines)
- **Complex packages**: Navigation hub with extensive references/

### Quality Assurance

- All code examples are runnable without modification
- Proper error handling in all examples
- Go best practices and idioms
- Security considerations for sensitive packages
- Links to official documentation

### Generated Output

Each generated skill includes:

- **SKILL.md**: Core usage patterns, common workflows, best practices
- **references/** (optional): Advanced topics for complex packages
- **Runnable examples**: Complete, self-contained code
- **Error handling**: Proper error patterns
- **Best practices**: Go-idiomatic usage

## Requirements

- Claude Code CLI or Claude Projects access
- Internet connection (for fetching pkg.go.dev documentation)

## License

[MIT](../LICENSE.md)
