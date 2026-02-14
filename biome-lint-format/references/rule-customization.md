# Rule Customization

Apply only the rule changes requested by the user.

## Enforce `===`

```json
{
  "linter": {
    "rules": {
      "suspicious": {
        "noDoubleEquals": "error"
      }
    }
  }
}
```

## Allow `console.log`

```json
{
  "linter": {
    "rules": {
      "suspicious": {
        "noConsoleLog": "off"
      }
    }
  }
}
```

## Require Explicit Return Types

```json
{
  "linter": {
    "rules": {
      "style": {
        "useExplicitType": "error"
      }
    }
  }
}
```

## Naming Convention Example

```json
{
  "linter": {
    "rules": {
      "style": {
        "useNamingConvention": {
          "level": "error",
          "options": {
            "strictCase": false
          }
        }
      }
    }
  }
}
```
