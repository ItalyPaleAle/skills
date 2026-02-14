# Skills

Custom [Skills](https://docs.claude.com/en/docs/claude-ai/projects#skills) for AI Agents like Claude, OpenAI, etc, organized by topic.

## Available Skills

| Skill | Description | Category |
|---|---|---|
| [react-spa-vite](./react-spa-vite/) | Scaffold and build production-ready static SPAs with Vite, React, Tailwind CSS v4, PWA support, SRI, and image optimization | Web Development |
| [biome-lint-format](./biome-lint-format/) | Set up Biome for fast linting and formatting in JavaScript/TypeScript projects | Tooling |
| [go-service-scaffolder](./go-service-scaffolder/) | Scaffold a production-ready Go HTTP service with OpenTelemetry observability, TLS, lifecycle management, Dockerfile, GitHub Actions CI/CD, and golangci-lint | Backend Development |

## Usage

### Using Claude Code CLI

The easiest way to add skills is using the [`skills`](https://skills.sh/) command:

```bash
npx skills add ItalyPaleAle/skills
```

This will automatically download and install the skill for use in your Claude Code projects.

### Using Claude Projects (claude.ai)

Add a skill to a Claude Project by including the raw GitHub URL in your Project knowledge:

```
https://raw.githubusercontent.com/ItalyPaleAle/skills/main/<skill-folder>/SKILL.md
```

Or download the `SKILL.md` file and upload it directly to your Project knowledge.

### Manual Installation

1. Navigate to the skill folder (e.g., `react-spa-vite/`)
2. Copy the `SKILL.md` file to your project's knowledge base or skills directory
3. Follow the instructions in the skill file

## License

[MIT](./LICENSE.md)
