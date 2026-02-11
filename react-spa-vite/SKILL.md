---
name: react-spa-vite
version: 1.0.0
description: Scaffold and build production-ready static SPAs with Vite, React, Tailwind v4, PWA support, SRI, and image optimization
category: web-development
tags:
  - react
  - vite
  - spa
  - pwa
  - tailwind
  - typescript
invocation:
  - "Create a React SPA with Vite"
  - "Set up a new React app with PWA support"
  - "Build a static React application"
  - "Initialize a Vite + React + Tailwind project"
examples:
  - "Create a production-ready React SPA with PWA support and Tailwind CSS"
  - "Set up a new Vite React app with image optimization and SRI"
  - "Build a static React application that works offline"
---

# Skill: Creating Static Single-Page Applications with Vite + React

## Overview

This skill teaches how to scaffold and build production-ready static Single-Page Applications (SPAs) using **Vite**, **React** (with SWC), **Tailwind CSS v4**, **PWA support**, **Subresource Integrity (SRI)**, and **image optimization**. The output is a static `dist/` folder suitable for deployment to any static hosting provider (Vercel, Netlify, Cloudflare Pages, S3, etc.).

## When to Use This Skill

Use this skill when you need to:

- Create a new React single-page application from scratch
- Set up a production-ready React project with modern tooling
- Build a PWA (Progressive Web App) that works offline
- Create a static site that can be deployed to any static hosting provider
- Set up a React project with performance optimizations (SRI, image optimization, etc.)

## Prerequisites

- Node.js 22+ installed
- pnpm package manager (recommended) or npm
- Basic knowledge of React and TypeScript

---

## Tech Stack

| Layer | Tool | Package |
|---|---|---|
| Build tool | Vite | `vite` |
| UI framework | React + TypeScript | `react`, `react-dom`, `@types/react`, `@types/react-dom` |
| JSX/TS compiler | SWC (via Vite plugin) | `@vitejs/plugin-react-swc` |
| Styling | Tailwind CSS v4 (Vite plugin) | `tailwindcss`, `@tailwindcss/vite` |
| PWA | vite-plugin-pwa | `vite-plugin-pwa` |
| SRI | vite-plugin-sri-gen | `vite-plugin-sri-gen` |
| Image optimization | vite-imagetools | `vite-imagetools` |

---

## Project Structure

```
my-app/
├── public/
│   ├── favicon.ico
│   ├── apple-touch-icon.png        # 180×180 PNG
│   └── icons/
│       ├── pwa-192x192.png
│       └── pwa-512x512.png
├── src/
│   ├── main.tsx                    # Application entry point
│   ├── App.tsx                     # Root component
│   ├── index.css                   # Global CSS (Tailwind import + @font-face)
│   ├── assets/
│   │   ├── fonts/                  # Self-hosted font files (.woff2, .woff)
│   │   └── images/                 # Source images for optimization
│   ├── components/                 # Reusable components
│   ├── hooks/                      # Custom hooks
│   ├── lib/                        # Utility functions
│   ├── pages/                      # Page-level components
│   └── vite-env.d.ts               # Vite client types
├── index.html                      # HTML entry point (in project root)
├── vite.config.ts
├── tsconfig.json
├── tsconfig.app.json
├── tsconfig.node.json
└── package.json
```

---

## 1. Initialization

```bash
pnpm create vite@latest my-app --template react-swc-ts
cd my-app
pnpm install
```

Then install the additional dependencies:

```bash
# Tailwind CSS v4 with its Vite plugin
pnpm add tailwindcss @tailwindcss/vite

# PWA support
pnpm add -D vite-plugin-pwa

# Subresource Integrity
pnpm add -D vite-plugin-sri-gen

# Image optimization
# Version 9+
pnpm add -D vite-imagetools
```

---

## 2. Vite Configuration

`vite.config.ts` — this is the central configuration file. **Plugin order matters.**

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react-swc";
import tailwindcss from "@tailwindcss/vite";
import { VitePWA } from "vite-plugin-pwa";
import { imagetools } from "vite-imagetools";
import sri from "vite-plugin-sri-gen";

export default defineConfig({
  plugins: [
    // 1. React with SWC for fast JSX/TS compilation and Fast Refresh
    react(),

    // 2. Tailwind CSS v4 — processes @import "tailwindcss" in CSS files
    tailwindcss(),

    // 3. Image optimization — transforms images at build time via import directives
    imagetools({
      defaultDirectives: (url) => {
        // Only apply defaults to images in src/assets/images
        // that don't already have explicit directives
        if (url.searchParams.size === 0) {
          return new URLSearchParams({
            format: "webp;avif",
          });
        }
        return new URLSearchParams();
      },
    }),

    // 4. PWA — generates service worker and web app manifest
    VitePWA({
      registerType: "autoUpdate",
      injectRegister: "inline",
      strategies: "generateSW",
      includeAssets: [
        "favicon.ico",
        "apple-touch-icon.png",
        "icons/*.png",
      ],
      manifest: {
        name: "My App",
        short_name: "MyApp",
        description: "A progressive web application",
        theme_color: "#ffffff",
        background_color: "#ffffff",
        display: "standalone",
        start_url: "/",
        icons: [
          {
            src: "/icons/pwa-192x192.png",
            sizes: "192x192",
            type: "image/png",
          },
          {
            src: "/icons/pwa-512x512.png",
            sizes: "512x512",
            type: "image/png",
          },
          {
            src: "/icons/pwa-512x512.png",
            sizes: "512x512",
            type: "image/png",
            purpose: "maskable",
          },
        ],
      },
      workbox: {
        globPatterns: ["**/*.{js,css,html,ico,png,svg,woff2,webmanifest}"],
        globIgnores: ["**/*.map", "**/manifest*.json", "**/*.LICENSE.txt", "**/icon*.png", "**/apple-touch-icon.png"],
        cleanupOutdatedCaches: true,
        clientsClaim: true,
        skipWaiting: true,
      },
    }),

    // 5. SRI — MUST be last so it hashes final output
    sri({
      algorithm: "sha384",
      crossorigin: "anonymous",
    }),
  ],
});
```

### Plugin Order Rules

1. `react()` — must come first; it handles JSX/TS transformation and HMR.
2. `tailwindcss()` — processes CSS; order relative to react doesn't strictly matter, but placing it second is conventional.
3. `imagetools()` — transforms image imports at build time; must run before output-modifying plugins.
4. `VitePWA()` — generates the service worker and manifest; place before SRI.
5. `sri()` — **must be last**. It hashes the final build output. Placing it before other plugins that modify output will produce incorrect hashes.

---

## 3. Tailwind CSS v4 Setup

Tailwind CSS v4 uses a fundamentally different setup than v3. There is **no `tailwind.config.js`**, **no `postcss.config.js`**, and **no `content` array** needed.

### `src/index.css`

```css
@import "tailwindcss";
```

That single line is all that's needed. Tailwind v4 automatically detects your template files.

### Customization in CSS (not JS)

All theme customization is done in CSS using `@theme`:

```css
@import "tailwindcss";

@theme {
  --color-primary: #3b82f6;
  --color-secondary: #10b981;
  --font-sans: "Inter", sans-serif;
}
```

### Key differences from Tailwind v3

- **No `tailwind.config.js`** — configure everything in CSS with `@theme`.
- **No `postcss.config.js`** — the `@tailwindcss/vite` plugin replaces PostCSS entirely.
- **No `content` array** — Tailwind v4 auto-detects template files.
- **No `@tailwind base/components/utilities` directives** — use `@import "tailwindcss"` instead.
- **CSS `@import` works natively** — no need for `postcss-import`.

---

## 4. HTML Entry Point

`index.html` — lives at the project root (not in `public/`). Vite uses this as the entry point.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="theme-color" content="#ffffff" />
    <link rel="icon" type="image/x-icon" href="/favicon.ico" />
    <link rel="apple-touch-icon" href="/apple-touch-icon.png" />
    <title>My App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```

### Important notes about `index.html`

- It must live at the **project root**, not inside `src/` or `public/`.
- The `<script>` tag must use `type="module"` and reference the source file directly — Vite handles transformation.
- Do **not** manually add `<link>` tags for CSS; Vite injects them automatically.
- Do **not** add `integrity` attributes manually; `vite-plugin-sri-gen` handles this at build time.

---

## 5. Application Entry Point

### `src/main.tsx`

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App";
import "./index.css";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

### `src/App.tsx`

```tsx
export default function App() {
  return (
    <div className="min-h-screen flex items-center justify-center bg-white">
      <h1 className="text-4xl font-bold text-gray-900">Hello World</h1>
    </div>
  );
}
```

---

## 6. TypeScript Configuration

### `tsconfig.json`

```json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ]
}
```

### `tsconfig.app.json`

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "useDefineForClassFields": true,
    "lib": ["ES2022", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["src"]
}
```

### `tsconfig.node.json`

```json
{
  "compilerOptions": {
    "target": "ES2025",
    "lib": ["ES2025"],
    "module": "ESNext",
    "skipLibCheck": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedSideEffectImports": true
  },
  "include": ["vite.config.ts"]
}
```

If using path aliases like `@/*`, also add the resolve alias to `vite.config.ts`:

```ts
import { resolve } from "path";

export default defineConfig({
  resolve: {
    alias: {
      "@": resolve(__dirname, "./src"),
    },
  },
  plugins: [/* ... */],
});
```

---

## 7. Vite Type Declarations

### `src/vite-env.d.ts`

```ts
/// <reference types="vite/client" />
```

If using `vite-plugin-pwa` with the virtual module for registration, also add:

```ts
/// <reference types="vite-plugin-pwa/client" />
```

For `vite-imagetools`, also add type declarations so TypeScript understands image imports with query parameters:

```ts
/// <reference types="vite-imagetools/client" />
```

---

## 8. PWA Details

### Service Worker Registration

With `registerType: "autoUpdate"`, the service worker auto-updates in the background. For manual control:

```ts
// src/sw-registration.ts
import { registerSW } from "virtual:pwa-register";

const updateSW = registerSW({
  onNeedRefresh() {
    // Show a prompt to the user asking to reload
    if (confirm("New content available. Reload?")) {
      updateSW(true);
    }
  },
  onOfflineReady() {
    console.log("App ready to work offline");
  },
});
```

### Workbox Caching Strategies

For API calls or runtime caching, extend `workbox.runtimeCaching`:

```ts
VitePWA({
  workbox: {
    runtimeCaching: [
      {
        urlPattern: /^https:\/\/api\.example\.com\/.*/i,
        handler: "NetworkFirst",
        options: {
          cacheName: "api-cache",
          networkTimeoutSeconds: 10,
          cacheableResponse: { statuses: [0, 200] },
        },
      },
      {
        urlPattern: /\.(?:png|jpg|jpeg|svg|gif|webp|avif)$/,
        handler: "CacheFirst",
        options: {
          cacheName: "image-cache",
          expiration: {
            maxEntries: 100,
            maxAgeSeconds: 30 * 24 * 60 * 60, // 30 days
          },
        },
      },
    ],
  },
})
```

### PWA Icons

At minimum, provide these icons in `public/icons/`:

- `pwa-192x192.png` — required by Chrome/Android
- `pwa-512x512.png` — required for splash screens and installability
- `apple-touch-icon.png` (180×180) — required for iOS home screen

---

## 9. SRI Details

`vite-plugin-sri-gen` runs **only at build time**. It:

- Adds `integrity` attributes to `<script>`, `<link rel="stylesheet">`, and `<link rel="modulepreload">` tags in the built HTML.
- Optionally injects `<link rel="modulepreload" integrity=...>` for lazy-loaded chunks (enabled by default via `preloadDynamicChunks`).
- Optionally injects a CSP-safe runtime that patches dynamically inserted `<script>`/`<link>` elements with integrity (enabled by default via `runtimePatchDynamicLinks`).
- Has **no effect during dev** — SRI is incompatible with HMR.

### Full SRI Options

```ts
sri({
  algorithm: "sha384",              // "sha256" | "sha384" | "sha512"
  crossorigin: "anonymous",         // "anonymous" | "use-credentials" | undefined
  fetchCache: true,                 // Cache remote asset fetches in-memory
  fetchTimeoutMs: 5000,             // Timeout for remote asset fetches (0 to disable)
  preloadDynamicChunks: true,       // Inject modulepreload for lazy chunks
  runtimePatchDynamicLinks: true,   // Patch dynamic script/link elements
  skipResources: [],                // Glob patterns to skip (e.g. analytics scripts)
})
```

### Skipping External Resources

```ts
sri({
  skipResources: [
    "https://www.googletagmanager.com/*",
    "*.googleapis.com/*",
    "analytics-script",  // matches element ID
  ],
})
```

---

## 10. Image Optimization with vite-imagetools

`vite-imagetools` transforms images **at build time** via import directives (query parameters on import paths). It uses Sharp under the hood.

### How It Works

Images are transformed when you import them in JS/TS code. Query parameters control the output:

```tsx
// Convert to webp
import heroWebp from "./assets/images/hero.jpg?format=webp";

// Convert to avif
import heroAvif from "./assets/images/hero.jpg?format=avif";

// Resize and convert
import thumb from "./assets/images/hero.jpg?w=400&format=webp";

// Generate multiple widths (returns an array of URLs)
import heroSrcset from "./assets/images/hero.jpg?w=480;960;1920&format=webp";
```

### Default Directives

The `defaultDirectives` option in the Vite config applies transformations to every image import that doesn't already have explicit query parameters. The configuration in section 2 sets `format=webp;avif` as the default, which generates both a WebP and AVIF version of every imported image.

If you want the default to apply only to specific directories, use the function form:

```ts
imagetools({
  defaultDirectives: (url) => {
    // Only auto-convert images in the assets/images directory
    if (url.pathname.includes("/assets/images/") && url.searchParams.size === 0) {
      return new URLSearchParams({ format: "webp;avif" });
    }
    return new URLSearchParams();
  },
}),
```

### Key Directives

| Directive | Example | Description |
|---|---|---|
| `format` | `?format=webp` | Convert to webp, avif, jpg, png, etc. |
| `w` / `width` | `?w=800` | Resize to width (height auto-scales) |
| `h` / `height` | `?h=600` | Resize to height (width auto-scales) |
| `quality` | `?format=webp&quality=80` | Set compression quality (1–100) |
| `aspect` | `?aspect=16:9` | Crop to aspect ratio |
| `fit` | `?w=800&h=600&fit=cover` | Fit mode: cover, contain, fill, inside, outside |
| `lossless` | `?format=webp&lossless` | Lossless encoding (webp, avif) |
| `effort` | `?format=avif&effort=4` | Compression effort (0=fast, max=slow/small) |
| `metadata` | `?w=800&format=webp&metadata` | Returns `{ src, width, height, format }` instead of just URL |

### Important Notes

- `vite-imagetools` only processes images imported in JS/TS code. It does **not** transform images in `public/` or images referenced only in HTML/CSS.
- During development, only metadata is generated (no actual image files) for speed. Actual image files are generated at build time.
- Images in `public/` are excluded by default and served as-is. Place source images in `src/assets/images/` instead.

---

## 11. Scripts

### `package.json` scripts

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview",
    "lint": "eslint ."
  },
  "packageManager": "pnpm@10"
  }
}
```

- `dev` — starts the Vite dev server at `localhost:5173` with HMR.
- `build` — type-checks with TypeScript, then builds to `dist/`.
- `preview` — serves the `dist/` folder locally to test the production build.

Use `pnpm dev`, `pnpm build`, and `pnpm preview` to run them.

---

## 12. Environment Variables

Vite exposes env variables prefixed with `VITE_` to client code.

### `.env`

```
VITE_API_URL=https://api.example.com
VITE_APP_TITLE=My App
```

### Usage in code

```ts
const apiUrl = import.meta.env.VITE_API_URL;
```

### Type safety

```ts
// src/vite-env.d.ts
interface ImportMetaEnv {
  readonly VITE_API_URL: string;
  readonly VITE_APP_TITLE: string;
}

interface ImportMeta {
  readonly env: ImportMetaEnv;
}
```

**Security note:** All `VITE_` variables are embedded in the client bundle and visible to users. Never put secrets in `VITE_` env vars.

---

## 13. Build Output

Running `npm run build` produces:

```
dist/
├── index.html              # With SRI integrity attributes injected
├── manifest.webmanifest    # PWA manifest (generated by vite-plugin-pwa)
├── sw.js                   # Service worker (generated by Workbox)
├── registerSW.js           # Service worker registration script
├── workbox-*.js            # Workbox runtime
└── assets/
    ├── index-[hash].js     # Bundled application JS
    ├── index-[hash].css    # Bundled CSS (Tailwind output)
    ├── [name]-[hash].woff2 # Font files (hashed for cache-busting)
    └── [name]-[hash].webp  # Optimized images
```

The `dist/` folder is entirely static and can be deployed anywhere.

---

## Common Patterns

### Self-hosting fonts

Always self-host fonts rather than loading them from Google Fonts or other external CDNs. This avoids extra DNS lookups, third-party requests, and potential privacy issues. It also ensures fonts are available offline when using PWA.

**Step 1:** Download `.woff2` files (and optionally `.woff` for legacy fallback) and place them in `src/assets/fonts/`:

```
src/assets/fonts/
├── Inter-Regular.woff2
├── Inter-Medium.woff2
├── Inter-SemiBold.woff2
└── Inter-Bold.woff2
```

**Step 2:** Declare `@font-face` rules in `src/index.css`, **before** the Tailwind import:

```css
@font-face {
  font-family: "Inter";
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("./assets/fonts/Inter-Regular.woff2") format("woff2");
}

@font-face {
  font-family: "Inter";
  font-style: normal;
  font-weight: 500;
  font-display: swap;
  src: url("./assets/fonts/Inter-Medium.woff2") format("woff2");
}

@font-face {
  font-family: "Inter";
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("./assets/fonts/Inter-SemiBold.woff2") format("woff2");
}

@font-face {
  font-family: "Inter";
  font-style: normal;
  font-weight: 700;
  font-display: swap;
  src: url("./assets/fonts/Inter-Regular.woff2") format("woff2");
}

@import "tailwindcss";

@theme {
  --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
}
```

**Key rules:**

- Always use `font-display: swap` to prevent invisible text while fonts load.
- Use relative paths (`./assets/fonts/...`) so Vite hashes and bundles the files into `dist/assets/`.
- Prefer `.woff2` — it has the best compression and is supported by all modern browsers.
- Do **not** place fonts in `public/` — they would bypass Vite's asset pipeline (no hashing, no cache-busting).
- Override Tailwind's `--font-sans` in `@theme` so `font-sans` utility class uses your custom font.
- Add `.woff2` to the PWA workbox `globPatterns` (already included in the config above) so fonts are cached for offline use.

### Importing static assets

```tsx
import logo from "./assets/logo.svg";

function Header() {
  return <img src={logo} alt="Logo" />;
}
```

### Optimized images with `<picture>` element

Use `vite-imagetools` to generate multiple formats and serve the best one with `<picture>`:

```tsx
import heroAvif from "./assets/images/hero.jpg?format=avif";
import heroWebp from "./assets/images/hero.jpg?format=webp";
import heroFallback from "./assets/images/hero.jpg";

function Hero() {
  return (
    <picture>
      <source type="image/avif" srcSet={heroAvif} />
      <source type="image/webp" srcSet={heroWebp} />
      <img src={heroFallback} alt="Hero image" width={1200} height={630} />
    </picture>
  );
}
```

### Responsive images with srcset

```tsx
// Returns an array of URLs when using multiple widths with semicolons
import heroSrcset from "./assets/images/hero.jpg?w=480;960;1440&format=webp";

function Hero() {
  return (
    <img
      srcSet={heroSrcset}
      sizes="(max-width: 480px) 480px, (max-width: 960px) 960px, 1440px"
      alt="Hero image"
    />
  );
}
```

### CSS Modules (alongside Tailwind)

```tsx
import styles from "./Button.module.css";

function Button({ children }: { children: React.ReactNode }) {
  return <button className={styles.button}>{children}</button>;
}
```

### Lazy loading routes

```tsx
import { lazy, Suspense } from "react";

const Dashboard = lazy(() => import("./pages/Dashboard"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  );
}
```

`vite-plugin-sri-gen` automatically handles SRI for lazy-loaded chunks via its `preloadDynamicChunks` and `runtimePatchDynamicLinks` features.

---

## 14. Testing (Optional)

### Unit & Component Testing with Vitest

[Vitest](https://vitest.dev) is a Vite-native test runner that's faster than Jest and requires minimal configuration.

#### Installation

```bash
pnpm add -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
```

#### Configuration

Add to `vite.config.ts`:

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react-swc";
// ... other imports

export default defineConfig({
  plugins: [/* ... */],
  test: {
    globals: true,
    environment: "jsdom",
    setupFiles: "./src/test/setup.ts",
    css: true,
  },
});
```

#### Test Setup File

Create `src/test/setup.ts`:

```ts
import { expect, afterEach } from "vitest";
import { cleanup } from "@testing-library/react";
import * as matchers from "@testing-library/jest-dom/matchers";

// Extend Vitest's expect with jest-dom matchers
expect.extend(matchers);

// Clean up after each test
afterEach(() => {
  cleanup();
});
```

#### Type Declarations

Add to `src/vite-env.d.ts`:

```ts
/// <reference types="vitest/globals" />
/// <reference types="@testing-library/jest-dom" />
```

#### Example Component Test

`src/components/Button.test.tsx`:

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { describe, it, expect, vi } from "vitest";
import Button from "./Button";

describe("Button", () => {
  it("renders with text", () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole("button")).toHaveTextContent("Click me");
  });

  it("calls onClick when clicked", async () => {
    const user = userEvent.setup();
    const handleClick = vi.fn();
    render(<Button onClick={handleClick}>Click me</Button>);

    await user.click(screen.getByRole("button"));
    expect(handleClick).toHaveBeenCalledOnce();
  });

  it("is disabled when disabled prop is true", () => {
    render(<Button disabled>Click me</Button>);
    expect(screen.getByRole("button")).toBeDisabled();
  });
});
```

#### Running Vitest

Add scripts to `package.json`:

```json
{
  "scripts": {
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage"
  }
}
```

- `pnpm test` — runs tests in watch mode
- `pnpm test:ui` — opens Vitest UI (requires `@vitest/ui`)
- `pnpm test:coverage` — generates coverage report (requires `@vitest/coverage-v8`)

#### Coverage Setup

For coverage reporting:

```bash
pnpm add -D @vitest/coverage-v8
```

Add to `vite.config.ts`:

```ts
export default defineConfig({
  test: {
    coverage: {
      provider: "v8",
      reporter: ["text", "json", "html"],
      exclude: [
        "node_modules/",
        "src/test/",
        "**/*.test.{ts,tsx}",
        "**/*.config.{ts,js}",
        "**/vite-env.d.ts",
      ],
    },
  },
});
```

---

### E2E Testing with Playwright

[Playwright](https://playwright.dev) provides cross-browser end-to-end testing.

#### Installation

```bash
pnpm create playwright
```

This interactive CLI will:

- Install `@playwright/test`
- Create `playwright.config.ts`
- Add example tests in `tests/` or `e2e/`
- Install browser binaries (Chromium, Firefox, WebKit)

#### Configuration

`playwright.config.ts`:

```ts
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  testDir: "./e2e",
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: "html",
  use: {
    baseURL: "http://localhost:5173",
    trace: "on-first-retry",
    screenshot: "only-on-failure",
  },

  projects: [
    {
      name: "chromium",
      use: { ...devices["Desktop Chrome"] },
    },
    {
      name: "firefox",
      use: { ...devices["Desktop Firefox"] },
    },
    {
      name: "webkit",
      use: { ...devices["Desktop Safari"] },
    },
    // Mobile viewports
    {
      name: "Mobile Chrome",
      use: { ...devices["Pixel 5"] },
    },
    {
      name: "Mobile Safari",
      use: { ...devices["iPhone 12"] },
    },
  ],

  // Start dev server before tests
  webServer: {
    command: "pnpm dev",
    url: "http://localhost:5173",
    reuseExistingServer: !process.env.CI,
  },
});
```

#### Example E2E Test

`e2e/homepage.spec.ts`:

```ts
import { test, expect } from "@playwright/test";

test.describe("Homepage", () => {
  test("should load and display heading", async ({ page }) => {
    await page.goto("/");

    // Check that the page loaded
    await expect(page).toHaveTitle(/My App/);

    // Check for main heading
    const heading = page.getByRole("heading", { level: 1 });
    await expect(heading).toBeVisible();
    await expect(heading).toHaveText("Hello World");
  });

  test("should be responsive", async ({ page }) => {
    await page.goto("/");

    // Mobile viewport
    await page.setViewportSize({ width: 375, height: 667 });
    await expect(page.getByRole("heading")).toBeVisible();

    // Desktop viewport
    await page.setViewportSize({ width: 1920, height: 1080 });
    await expect(page.getByRole("heading")).toBeVisible();
  });
});
```

#### Testing PWA Features

`e2e/pwa.spec.ts`:

```ts
import { test, expect } from "@playwright/test";

test.describe("PWA", () => {
  test("should register service worker", async ({ page }) => {
    await page.goto("/");

    // Wait for service worker registration
    const swRegistered = await page.evaluate(() => {
      return new Promise((resolve) => {
        if ("serviceWorker" in navigator) {
          navigator.serviceWorker.ready.then(() => resolve(true));
        } else {
          resolve(false);
        }
      });
    });

    expect(swRegistered).toBe(true);
  });

  test("should have web manifest", async ({ page }) => {
    await page.goto("/");

    const manifestLink = page.locator('link[rel="manifest"]');
    await expect(manifestLink).toHaveAttribute("href", /manifest\.webmanifest/);
  });

  test("should work offline", async ({ page, context }) => {
    // First visit to cache assets
    await page.goto("/");
    await page.waitForLoadState("networkidle");

    // Go offline
    await context.setOffline(true);

    // Navigate to home (should load from cache)
    await page.reload();
    await expect(page.getByRole("heading")).toBeVisible();

    // Go back online
    await context.setOffline(false);
  });
});
```

#### Running Playwright Tests

Add to `package.json`:

```json
{
  "scripts": {
    "test:e2e": "playwright test",
    "test:e2e:ui": "playwright test --ui",
    "test:e2e:headed": "playwright test --headed",
    "test:e2e:debug": "playwright test --debug"
  }
}
```

- `pnpm test:e2e` — runs all E2E tests headlessly
- `pnpm test:e2e:ui` — opens Playwright UI mode
- `pnpm test:e2e:headed` — runs tests with browser visible
- `pnpm test:e2e:debug` — runs tests in debug mode with step-through

#### Testing Production Build

To test the production build instead of dev server, update `playwright.config.ts`:

```ts
export default defineConfig({
  webServer: {
    command: "pnpm preview",
    url: "http://localhost:4173",
    reuseExistingServer: !process.env.CI,
  },
  use: {
    baseURL: "http://localhost:4173",
  },
});
```

Run `pnpm build` before running E2E tests in this mode.

---

## 15. Troubleshooting

### Build Issues

#### TypeScript Errors During Build

**Problem:** `pnpm build` fails with TypeScript errors.

**Solution:**

```bash
# Check which files have errors
pnpm tsc -b --listFiles

# Run TypeScript check separately to see detailed errors
pnpm tsc -b --noEmit
```

Common causes:

- Missing type declarations for imported packages — install `@types/*` packages
- Incorrect `tsconfig.json` paths or includes
- Using `any` types with strict mode enabled

---

#### Vite Build Fails with "Out of Memory"

**Problem:** Build process crashes with heap memory error.

**Solution:**

```bash
# Increase Node.js memory limit
NODE_OPTIONS=--max-old-space-size=4096 pnpm build
```

Or add to `package.json`:

```json
{
  "scripts": {
    "build": "NODE_OPTIONS=--max-old-space-size=4096 vite build"
  }
}
```

---

### Plugin Issues

#### SRI: Integrity Mismatch or Not Working

**Problem:** SRI `integrity` attributes not appearing in built HTML, or CSP errors in browser.

**Causes & Solutions:**

1. **SRI plugin not last in plugin array**
   - `vite-plugin-sri-gen` MUST be the last plugin in `vite.config.ts`
   - Other plugins that modify output (e.g., compression plugins) must come before SRI

2. **SRI doesn't work in dev mode**
   - SRI is build-time only and incompatible with HMR
   - Always test SRI with `pnpm build && pnpm preview`

3. **External scripts/links not getting integrity**
   - External resources are skipped by default for performance
   - To include external assets, ensure they're CORS-enabled and accessible at build time

---

#### PWA: Service Worker Not Updating

**Problem:** Old service worker persists after deploying new version.

**Solution:**

```ts
// Switch from autoUpdate to prompt mode
VitePWA({
  registerType: "prompt",
  // ... rest of config
})
```

Then implement update prompt in your app:

```tsx
import { useRegisterSW } from "virtual:pwa-register/react";

function App() {
  const {
    needRefresh: [needRefresh, setNeedRefresh],
    updateServiceWorker,
  } = useRegisterSW({
    onNeedRefresh() {
      // Show update prompt to user
    },
  });

  return (
    <>
      {needRefresh && (
        <div className="update-banner">
          New version available!
          <button onClick={() => updateServiceWorker(true)}>
            Update
          </button>
        </div>
      )}
      {/* rest of app */}
    </>
  );
}
```

**Hard Reset (for development):**

1. Open DevTools → Application → Service Workers
2. Click "Unregister" on the service worker
3. Clear site data: Application → Storage → Clear site data
4. Hard refresh: Cmd+Shift+R (Mac) or Ctrl+Shift+R (Windows/Linux)

---

#### PWA: Assets Not Cached

**Problem:** Service worker registered but assets not available offline.

**Solution:**

1. **Check `globPatterns` includes all file types:**

   ```ts
   VitePWA({
     workbox: {
       globPatterns: ["**/*.{js,css,html,ico,png,svg,woff2,webp,avif,webmanifest}"],
     },
   })
   ```

2. **Verify assets are in `dist/` after build:**

   ```bash
   pnpm build
   ls -R dist/
   ```

3. **Check service worker cache in DevTools:**
   - Application → Cache Storage → workbox-precache-*
   - Verify your assets are listed

---

#### Tailwind: Styles Not Applied

**Problem:** Tailwind classes not working or not generating styles.

**Causes & Solutions:**

1. **Wrong import in CSS file**
   - Tailwind v4 uses `@import "tailwindcss";` not `@tailwind` directives
   - Make sure `src/index.css` has the correct import

2. **CSS file not imported in `main.tsx`**

   ```tsx
   import "./index.css"; // Must be imported
   ```

3. **Plugin order issue**
   - `tailwindcss()` plugin must come after `react()` or at least early in the plugin array

4. **Purge issue (rare in v4)**
   - Tailwind v4 auto-detects files, but if styles are missing, ensure your template files use standard extensions (`.tsx`, `.jsx`, `.html`)

---

#### Image Optimization: Images Not Transforming

**Problem:** Images not being optimized or query parameters ignored.

**Causes & Solutions:**

1. **Images in `public/` directory**
   - `vite-imagetools` only processes images imported in JS/TS code
   - Move images to `src/assets/images/` and import them:

   ```tsx
   import heroWebp from "./assets/images/hero.jpg?format=webp";
   ```

2. **Missing query parameters**
   - Images without query params or `defaultDirectives` are not transformed
   - Either add `?format=webp` to imports or configure `defaultDirectives`

3. **Sharp installation issues**
   - `vite-imagetools` uses Sharp, which requires native binaries
   - If build fails, try reinstalling Sharp:

   ```bash
   pnpm remove sharp
   pnpm add -D sharp
   ```

4. **TypeScript errors on image imports**
   - Add type declarations in `src/vite-env.d.ts` (see section 7)

---

#### Path Aliases Not Resolving

**Problem:** `@/components/Button` imports fail with module not found error.

**Solution:**

1. **Add to `vite.config.ts`:**

   ```ts
   import { resolve } from "path";

   export default defineConfig({
     resolve: {
       alias: {
         "@": resolve(__dirname, "./src"),
       },
     },
   });
   ```

2. **Ensure `tsconfig.app.json` has matching paths:**

   ```json
   {
     "compilerOptions": {
       "baseUrl": ".",
       "paths": {
         "@/*": ["./src/*"]
       }
     }
   }
   ```

3. **Restart TypeScript server** in your editor (VS Code: Cmd+Shift+P → "TypeScript: Restart TS Server")

---

### Development Issues

#### HMR Not Working

**Problem:** Changes to files don't trigger hot reload.

**Solutions:**

1. **Check Vite dev server logs** for errors
2. **Ensure `react()` plugin is first** in plugin array
3. **Disable browser extensions** that might interfere with WebSocket connections
4. **Check firewall/proxy** isn't blocking WebSocket connection to `localhost:5173`
5. **Restart dev server:**

   ```bash
   # Kill all node processes if needed
   killall node
   pnpm dev
   ```

---

#### Slow Dev Server Startup

**Problem:** `pnpm dev` takes a long time to start.

**Solutions:**

1. **Optimize dependencies** — pre-bundle heavy dependencies:

   ```ts
   export default defineConfig({
     optimizeDeps: {
       include: ["react", "react-dom", "react-router-dom"],
     },
   });
   ```

2. **Exclude large directories:**

   ```ts
   export default defineConfig({
     server: {
       watch: {
         ignored: ["**/node_modules/**", "**/dist/**", "**/.git/**"],
       },
     },
   });
   ```

3. **Disable source maps in dev** (faster but harder to debug):

   ```ts
   export default defineConfig({
     build: {
       sourcemap: false,
     },
   });
   ```

---

## Checklist Before Shipping

- [ ] All `public/icons/` PWA icons are present (192×192, 512×512, apple-touch-icon)
- [ ] `manifest` in VitePWA config has correct `name`, `short_name`, `theme_color`
- [ ] Fonts are self-hosted in `src/assets/fonts/` with `@font-face` declarations (no external font CDN)
- [ ] Images in `src/assets/images/` are imported in code (not in `public/`) so they are optimized
- [ ] `npm run build` succeeds without TypeScript errors
- [ ] `npm run preview` works and the app loads correctly
- [ ] Service worker registers (check DevTools → Application → Service Workers)
- [ ] SRI `integrity` attributes appear on `<script>` and `<link>` tags in built HTML
- [ ] No secrets in `VITE_` environment variables
