# Package Analysis Guide

Guide for determining appropriate detail level and structure for Go package skills.

## Complexity Assessment

### Indicators of Simple Package

- Single primary type or function
- <10 exported functions
- One main use case
- Minimal configuration
- Examples: `github.com/google/uuid`, `github.com/rs/cors`

**Approach**: Single SKILL.md, 200-400 lines, focus on the main pattern.

### Indicators of Medium Package

- 2-5 main types
- 10-30 exported functions
- Multiple related use cases
- Some configuration options
- Examples: `github.com/go-chi/chi`, `github.com/lib/pq`

**Approach**: SKILL.md (300-600 lines) with optional references/ for advanced topics.

### Indicators of Complex Package

- Framework-like structure
- >30 exported functions
- Many distinct use cases
- Extensive configuration
- Multiple subpackages
- Examples: `github.com/gin-gonic/gin`, `google.golang.org/grpc`

**Approach**: SKILL.md as navigation hub, extensive references/ organized by topic.

## Package Analysis Checklist

When analyzing a Go package, extract:

### 1. Core Purpose
- What problem does it solve?
- What domain does it operate in?
- One-sentence summary

### 2. Main Types
- Primary types users interact with
- Their relationships
- Lifecycle (creation, usage, disposal)

### 3. Entry Points
- Most common function/method to start with
- Constructor patterns (`New`, `NewWith`, etc.)
- Builder patterns if any

### 4. Common Patterns
From documentation and examples, identify:
- The "hello world" pattern (simplest usage)
- 2-3 most common real-world patterns
- Configuration patterns
- Error handling patterns

### 5. Critical Context
- Thread safety notes
- Performance considerations
- Common pitfalls from documentation
- Integration with stdlib or other packages

### 6. Subpackages
- Are there important subpackages?
- Should they be separate sections or separate skills?
- Which are essential vs. optional?

## Documentation Sources

### pkg.go.dev Page Structure

When fetching from `https://pkg.go.dev/<import-path>`:

1. **Overview section**: Package-level documentation (most important)
2. **Index**: Lists all types and functions (scan for main types)
3. **Examples**: Official examples (use these!)
4. **Subdirectories**: Subpackages (decide which to include)

### What to Extract

Priority order:
1. Package overview (from README or doc.go)
2. Example code (official examples are gold)
3. Main type documentation
4. Constructor and method signatures
5. Subpackage purposes

### What to Skip

- Implementation details
- Deprecated APIs
- Internal packages
- Full type definitions (link instead)

## Content Organization Patterns

### Pattern 1: Single Responsibility Package

```markdown
# Package Name
<Quick intro>

## Usage
<One primary example>

## Configuration
<Key options>

## Best Practices
<3-5 bullets>
```

Example: A UUID generator package

### Pattern 2: Multiple Workflows Package

```markdown
# Package Name
<Quick intro>

## Quick Start
<Simplest example>

## Common Workflows

### Workflow 1: <Name>
<Example>

### Workflow 2: <Name>
<Example>

## Configuration
## Error Handling
## Best Practices
```

Example: An HTTP router package

### Pattern 3: Framework Package

```markdown
# Package Name
<Overview>

## Quick Start
<Basic server example>

## Core Concepts
<Essential understanding>

## Common Tasks

### Task 1: <Name>
<Link to references/task1.md>

### Task 2: <Name>
<Link to references/task2.md>

## Reference Map
<Navigation to references/>
```

Example: A web framework

## Quality Checks

Before finalizing the skill:

### Description Quality
- [ ] Describes what package does in one sentence
- [ ] Lists 2-3 primary use cases
- [ ] Includes clear triggers (when to use)
- [ ] Mentions package name variations users might say

### Content Quality
- [ ] All code examples compile
- [ ] Examples include error handling
- [ ] Imports are correct and complete
- [ ] Examples are self-contained
- [ ] No obvious security issues in examples

### Structure Quality
- [ ] SKILL.md under 500 lines (or content split to references/)
- [ ] Clear navigation if using references/
- [ ] No README.md or auxiliary docs
- [ ] YAML frontmatter valid with name and description only

### Coverage Quality
- [ ] Most common use case covered
- [ ] Error handling explained
- [ ] 3-5 best practices listed
- [ ] Links to pkg.go.dev for full API reference
- [ ] Critical pitfalls mentioned

## Special Cases

### Case: Package with Many Subpackages

Example: `github.com/aws/aws-sdk-go-v2`

**Strategy**: Either create separate skills per major service, or create one skill with references/ per service category.

### Case: Replacement for Stdlib

Example: `github.com/valyala/fasthttp` (replacement for net/http)

**Strategy**: Focus on differences from stdlib, migration patterns, and performance benefits. Assume knowledge of stdlib.

### Case: Wrapper Package

Example: Package wrapping a C library

**Strategy**: Focus on Go-idiomatic usage, hide C details, emphasize error handling and cleanup.

### Case: Crypto/Security Package

Example: `github.com/lestrrat-go/jwx`

**Strategy**: Emphasize security best practices, show secure defaults, warn about common vulnerabilities, include validation patterns.

## Examples of Good Structure

### Simple: UUID Package

```
uuid/
└── SKILL.md (200 lines)
    ├── What UUIDs are
    ├── Generating V4 UUIDs
    ├── Parsing and validating
    └── Best practices
```

### Medium: HTTP Router

```
chi/
├── SKILL.md (400 lines)
│   ├── Quick start
│   ├── Route definition patterns
│   ├── Middleware usage
│   └── Link to references for advanced topics
└── references/
    └── advanced-routing.md
```

### Complex: Web Framework

```
gin/
├── SKILL.md (300 lines - navigation hub)
│   ├── Quick start
│   ├── Core concepts
│   └── Task navigation
└── references/
    ├── routing.md
    ├── middleware.md
    ├── validation.md
    └── deployment.md
```
