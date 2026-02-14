# Migration from ESLint + Prettier

## 1. Remove Old Tooling

```bash
pnpm remove eslint prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin eslint-config-prettier eslint-plugin-prettier
```

Remove old config files if present:
- `.eslintrc.*`
- `.prettierrc*`
- `.prettierignore`

## 2. Install and Initialize Biome

```bash
pnpm add -D @biomejs/biome
pnpm biome init
```

Replace the generated config with your project baseline.

## 3. Replace Scripts

Use Biome scripts (`lint`, `format`, `check`, `ci`) and remove ESLint/Prettier scripts.

## 4. Apply First Auto-fix Pass

```bash
pnpm check:fix
```

Review changes carefully before committing.

## 5. Update CI

```bash
pnpm biome ci .
```

## 6. Hybrid Mode (If Needed)

If specific ESLint plugins are still required, run both tools temporarily:

```json
{
  "scripts": {
    "lint": "biome check . && eslint .",
    "lint:fix": "biome check --write . && eslint --fix ."
  }
}
```

Keep hybrid mode temporary and remove once equivalent coverage is available.
