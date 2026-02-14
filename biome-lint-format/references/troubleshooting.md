# Troubleshooting

## Biome Config Not Detected

- Ensure `biome.json` is in project root.
- Validate JSON syntax.
- Run `pnpm biome check biome.json`.

## Format on Save Not Working

- Confirm Biome VS Code extension is installed/enabled.
- Confirm `editor.defaultFormatter` is `biomejs.biome` for target languages.
- Disable conflicting workspace formatters.

## Schema Validation Errors

- Match `$schema` URL version with installed Biome version.
- Check with `pnpm biome --version`.

## Type Errors Missing

Biome is not a replacement for TypeScript type checking.

Add typecheck scripts when needed:

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "validate": "pnpm typecheck && pnpm check"
  }
}
```

## Performance on Large Repos

- Keep `vcs.useIgnoreFile: true`.
- Ignore heavy build artifacts in `files.ignore`.
- Prefer staged-file checks in pre-commit hooks.
