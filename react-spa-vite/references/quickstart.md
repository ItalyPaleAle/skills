# Quickstart

## 1. Initialize Project

```bash
pnpm create vite@latest my-app --template react-swc-ts
cd my-app
pnpm install
```

## 2. Install Core Dependencies

```bash
pnpm add tailwindcss @tailwindcss/vite
pnpm add -D vite-plugin-pwa vite-plugin-sri-gen vite-imagetools
```

Install optional testing tools only when requested:

```bash
pnpm add -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event jsdom
pnpm create playwright
```

## 3. Baseline Structure

```text
my-app/
├── public/
│   ├── favicon.ico
│   ├── apple-touch-icon.png
│   └── icons/
├── src/
│   ├── main.tsx
│   ├── App.tsx
│   ├── index.css
│   ├── assets/
│   │   ├── fonts/
│   │   └── images/
│   └── vite-env.d.ts
├── index.html
├── vite.config.ts
├── tsconfig.json
├── tsconfig.app.json
└── tsconfig.node.json
```

## 4. Baseline Scripts

```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview"
  }
}
```

## 5. Validate

```bash
pnpm build
pnpm preview
```

The build output is a static `dist/` folder suitable for static hosting providers.
