# React SPA with Vite Skill

This skill provides comprehensive guidance for scaffolding and building production-ready static Single-Page Applications (SPAs) using:

- **Vite** - Fast build tool
- **React** - Latest React with TypeScript
- **Tailwind CSS v4** - Modern utility-first CSS
- **PWA Support** - Offline-capable progressive web apps
- **Subresource Integrity (SRI)** - Enhanced security
- **Image Optimization** - Automatic image transformation
- **Vitest** - Fast unit testing (optional)
- **Playwright** - E2E testing (optional)

## What This Skill Covers

1. Project initialization and setup
2. Vite configuration with all plugins
3. Tailwind CSS v4 setup (CSS-only configuration)
4. TypeScript configuration
5. PWA setup with service workers
6. SRI configuration for security
7. Image optimization with vite-imagetools
8. Self-hosting fonts
9. Environment variables
10. Testing setup (unit and E2E)
11. Common patterns and best practices
12. Troubleshooting guide

## Installation

### Using Claude Code CLI

```bash
npx skills add https://github.com/ItalyPaleAle/skills/tree/main/react-spa-vite
```

### Using Claude Projects (claude.ai)

Add the following URL to your Project knowledge:

```
https://raw.githubusercontent.com/ItalyPaleAle/skills/main/react-spa-vite/SKILL.md
```

### Manual Installation

Download [SKILL.md](./SKILL.md) and add it to your Claude Code project or Claude Project knowledge base.

## Usage

Once installed, you can invoke this skill by asking Claude to:

- "Create a React SPA with Vite"
- "Set up a new React app with PWA support"
- "Build a static React application"
- "Initialize a Vite + React + Tailwind project"

Claude will use this skill to guide you through the complete setup process with best practices and proper configuration.

## Features

### Modern Stack

- React 19 with TypeScript
- Vite with SWC for fast compilation
- Tailwind CSS v4 (no config file needed)
- Path aliases (`@/components`)

### Production Ready

- PWA with service worker and offline support
- Subresource Integrity for enhanced security
- Image optimization (WebP, AVIF)
- Self-hosted fonts (no external CDN)
- Environment variable management

### Developer Experience

- Hot Module Replacement (HMR)
- TypeScript strict mode
- Vitest for unit testing
- Playwright for E2E testing
- Comprehensive troubleshooting guide

### Build Output

The build produces a fully static `dist/` folder that can be deployed to:

- Vercel
- Netlify
- Cloudflare Pages
- AWS S3 + CloudFront
- GitHub Pages
- Any static hosting provider

## Requirements

- Node.js 22+
- pnpm (recommended) or npm
- Basic knowledge of React and TypeScript

## License

[MIT](../LICENSE.md)
