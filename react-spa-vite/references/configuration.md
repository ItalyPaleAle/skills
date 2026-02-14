# Configuration

## Vite Plugin Baseline

`vite.config.ts` plugin order:
1. `react()`
2. `tailwindcss()`
3. `imagetools()`
4. `VitePWA()`
5. `sri()` (must be last)

Minimal pattern:

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react-swc';
import tailwindcss from '@tailwindcss/vite';
import { VitePWA } from 'vite-plugin-pwa';
import { imagetools } from 'vite-imagetools';
import sri from 'vite-plugin-sri-gen';

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(),
    imagetools(),
    VitePWA({ registerType: 'autoUpdate' }),
    sri({ algorithm: 'sha384', crossorigin: 'anonymous' }),
  ],
});
```

## Tailwind CSS v4

Use CSS-first setup (no `tailwind.config.js` required by default):

```css
@import "tailwindcss";

@theme {
  --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
}
```

## Entry Points

- Keep `index.html` at project root.
- Keep app bootstrapping in `src/main.tsx`.
- Import `src/index.css` from `src/main.tsx`.

## TypeScript

Use strict settings in `tsconfig.app.json` and add aliases only when needed.

Alias example:

```ts
resolve: {
  alias: {
    '@': resolve(__dirname, './src')
  }
}
```

## PWA

Recommended defaults:
- `registerType: "autoUpdate"`
- include app icons and favicon assets
- enable cleanup and immediate activation in Workbox

Provide at minimum:
- `public/icons/pwa-192x192.png`
- `public/icons/pwa-512x512.png`
- `public/apple-touch-icon.png`

## SRI

Use `vite-plugin-sri-gen` only for build output; it has no effect in dev.

Rules:
- Keep it last in plugin order.
- Verify `integrity` attributes by running `pnpm build && pnpm preview`.

## Image Optimization

Use `vite-imagetools` for images imported from source code (`src/assets/images`).

Example:

```tsx
import heroAvif from './assets/images/hero.jpg?format=avif';
import heroWebp from './assets/images/hero.jpg?format=webp';
```

Avoid using `public/` for images you want transformed.

## Environment Variables

- Expose only `VITE_*` variables to client code.
- Never store secrets in client-exposed env vars.

## Self-hosted Fonts

Keep font files in `src/assets/fonts` and declare `@font-face` in CSS with `font-display: swap`.
