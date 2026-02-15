# Go Package Skill Template

Use this template as a starting point for Go package skills.

## SKILL.md Structure

```markdown
---
name: <package-name>
description: <What the package does>. <Primary use cases>. Use when <specific triggers: working with X, implementing Y, user mentions package name>.
---

# <Package Name>

<One sentence describing what the package provides>

## Core Usage

<The most common usage pattern with a complete, runnable example>

### Basic Example

```go
package main

import (
    "<import-path>"
)

func main() {
    // Show the most common use case
    // Include error handling
    // Keep it minimal but complete
}
```

## Common Patterns

### Pattern 1: <Name>

<When to use this pattern>

```go
// Complete example showing the pattern
```

### Pattern 2: <Name>

<When to use this pattern>

```go
// Complete example showing the pattern
```

## Error Handling

<Common errors and how to handle them>

```go
// Example showing proper error handling
```

## Best Practices

- <Practice 1>
- <Practice 2>
- <Practice 3>

## Advanced Topics (Optional)

<Only include if package is complex>

For <advanced topic>, see `references/advanced.md`.

## Key Types and Functions

<Brief reference of essential types/functions - not exhaustive API docs>

- `TypeName`: <purpose>
- `FunctionName()`: <purpose>

## Structure Guidelines

### Simple Package (< 500 lines)

Single SKILL.md with:
- Core usage (one primary example)
- 2-3 common patterns
- Error handling
- 3-5 best practices

### Medium Package (500-1000 lines)

SKILL.md with:
- Quick start example
- Common patterns (3-5)
- Error handling section
- Links to references/ for advanced topics

references/ might include:
- `advanced.md` - Complex use cases
- `examples.md` - Additional examples

### Complex Package (> 1000 lines)

SKILL.md with:
- Overview and navigation
- Essential patterns only
- Clear links to references/

references/ organized by topic:
- `auth.md` - Authentication patterns
- `validation.md` - Validation usage
- `examples.md` - Comprehensive examples

## Content Principles

### Do Include

- **Working examples**: Every code block should be runnable
- **Error handling**: Show how to handle common errors
- **Context**: Explain *when* to use each pattern
- **Common pitfalls**: Warn about mistakes
- **Integration**: How to use with other packages

### Don't Include

- **Full API reference**: Link to pkg.go.dev instead
- **Package history**: Focus on current version
- **Comparisons**: Don't compare to other packages unless essential
- **Installation**: Assume user knows `go get`
- **Basics**: Assume Go competence (don't explain goroutines, etc.)

## Example Code Quality

All examples must:
- Compile without errors
- Include necessary imports
- Handle errors properly
- Be self-contained (runnable as-is)
- Use realistic variable names
- Include comments for non-obvious logic

Bad example:
```go
jwt.Parse(token) // might not compile, no error handling
```

Good example:
```go
import "github.com/golang-jwt/jwt/v5"

func parseToken(tokenString string, secret []byte) (*jwt.Token, error) {
    token, err := jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
        return secret, nil
    })
    if err != nil {
        return nil, fmt.Errorf("failed to parse token: %w", err)
    }
    return token, nil
}
```

## Description Formula

```
description: <Package purpose in 1 sentence>. <Key capabilities/features>. Use when <trigger 1>, <trigger 2>, or <trigger 3>.
```

Example:
```
description: JWT token creation, parsing, and validation for Go with support for standard claims and custom signing methods. Provides secure token-based authentication and authorization. Use when implementing JWT authentication, validating API tokens, creating secure sessions, or when the user mentions JWT or token authentication in Go.
```

## Frontmatter Only

Only include `name` and `description` in YAML frontmatter. No other fields.
