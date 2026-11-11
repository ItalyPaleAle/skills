# Cut Release Skill

This skill prepares and ships releases for Ale's GitHub projects, including `italypaleale/traefik-forward-auth`, `francis`, and `revaulter`, using signed, annotated Git tags. The tag message contains Ale's final release notes, and pushing the tag triggers the repository's release workflow.

## What This Skill Covers

1. Repository, default branch, working tree, and release workflow checks
2. Stable and pre-release tag discovery, including first releases
3. Commit collection grouped into Security, New features, Fixes, and Changes
4. Contributor attribution and dependency updates, including security fixes
5. Version choices, pre-release numbering, and promotion to stable
6. Signed tag creation with the final release notes preserved verbatim
7. Tag verification and an explicit approval before pushing

## Installation

### Using Claude Code CLI

```bash
npx skills add https://github.com/ItalyPaleAle/skills/tree/main/cut-release
```

### Using Claude Projects (claude.ai)

Add the following URL to your Project knowledge:

```
https://raw.githubusercontent.com/ItalyPaleAle/skills/main/cut-release/SKILL.md
```

### Manual Installation

Download [SKILL.md](./SKILL.md) and add it to your agent's skills directory or Project knowledge base.

## Usage

Run the agent in the repository you want to release and ask it to:

- "Prepare a release of this project"
- "List the changes since the last stable release"
- "Prepare the next release candidate"
- "Promote the latest release candidate to a stable release"

The agent gathers and groups changes for review. Ale chooses the version and supplies the final release notes. The agent then creates and verifies the signed tag, presents the tag and target commit, and asks for explicit approval before pushing.

## Release Behavior

- Stable releases cover changes since the last stable tag, including the full release candidate cycle.
- Pre-releases cover changes since the preceding release tag. First releases cover the full history.
- The reviewed commit stays fixed; new commits require another review before tagging or pushing.
- Tag messages preserve the final notes, including Markdown headings, without added titles or attribution.
- Only the selected tag is pushed. Tags are never force-pushed, deleted, or moved.
- The skill leaves working tree files unchanged and does not create commits, PRs, issues, or comments.
- GitHub Actions builds and publishes the release. The skill does not create a GitHub release directly or wait for CI unless asked.

## Requirements

- Git and access to fetch from and push tags to the repository's `origin` remote
- A clean working tree on the default branch, matching its remote commit
- Git tag signing already configured, with access to the signing key; existing projects use SSH signatures
- A GitHub Actions workflow triggered by `v*` tags that uses the tag message as release notes and handles pre-release suffixes
- Optional: GitHub CLI (`gh`) for contributor handles and workflow status

An agent with shell access is needed to run the Git commands. Project knowledge alone supplies the instructions.

## License

[MIT](../LICENSE.md)
