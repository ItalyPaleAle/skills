# Troubleshooting

## Build Fails with TypeScript Errors

- Run `pnpm tsc -b --noEmit` for clear diagnostics.
- Verify `tsconfig` includes and path aliases.

## SRI Not Applied

- Ensure `sri()` is last in Vite plugins.
- Check only with production build (`pnpm build && pnpm preview`).

## Service Worker Not Updating

- Consider `registerType: "prompt"` and manual refresh UX.
- Clear service worker/site data during local debugging.

## PWA Assets Missing Offline

- Ensure `workbox.globPatterns` include required extensions.
- Confirm assets exist in `dist/` after build.

## Tailwind Styles Missing

- Confirm `@import "tailwindcss";` in `src/index.css`.
- Confirm `src/index.css` is imported in `src/main.tsx`.

## Image Transforms Not Running

- Import images from `src/assets/images` (not `public/`).
- Include directives like `?format=webp` or set defaults in `imagetools()`.

## Alias Resolution Errors

- Keep Vite `resolve.alias` and TypeScript `paths` in sync.

## Slow/Unstable Dev Server

- Review heavy dependencies and Vite optimize deps.
- Restart dev server after major config changes.
