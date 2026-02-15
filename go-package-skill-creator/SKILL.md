---
name: go-package-skill-creator
description: Create skills for Go packages by fetching documentation from pkg.go.dev and generating structured SKILL.md files with usage patterns, examples, and best practices. Use when the user wants to create a skill for a specific Go package (e.g., "create a skill for github.com/lestrrat-go/jwx/v3") or needs to document how to use a Go library.
---

# Go Package Skill Creator

Generate skills that teach how to use specific Go packages.

## Workflow

1. **Gather information**:
   - Package import path (e.g., `github.com/lestrrat-go/jwx/v3`)
   - Additional examples or use cases (optional)
   - Any specific aspects to emphasize (optional)

2. **Fetch documentation**:
   - Get package documentation from `https://pkg.go.dev/<import-path>`
   - Extract: package overview, main types, key functions, and code examples
   - If the package has multiple subpackages, fetch the most relevant ones

3. **Analyze package complexity**:
   - **Simple** (single purpose, <10 key functions): Focus on core usage pattern with 1-2 examples
   - **Medium** (multiple types, 10-30 functions): Include common patterns and error handling
   - **Complex** (framework-like, >30 functions): Split into sections, consider references/ for advanced topics

4. **Derive skill identity**:
   - **Name**: Use `go-` and the last segment of import path (e.g., `jwx` from `github.com/lestrrat-go/jwx/v3`)
     - If name is too generic (like `http` or `json`), prepend with package scope (e.g., `go-fasthttp`)
   - **Description**: Include:
     - What the package does (1 sentence)
     - Primary use cases (2-3 bullet points)
     - When to trigger: "Use when working with [domain], [specific task], or when the user mentions [package name]"

5. **Create skill structure**:
   ```
   <package-name>/
   ├── SKILL.md
   └── references/        (optional, only if complex)
       └── advanced.md
   ```

6. **Write SKILL.md** following the template in `references/skill-template.md`:
   - Keep under 500 lines if possible
   - Focus on practical patterns over API reference
   - Include runnable examples
   - For complex packages, move advanced topics to references/

7. **Create the skill directory and files**:
   - Use Write tool to create SKILL.md
   - Create any necessary reference files
   - No README.md or auxiliary documentation

8. **Validate structure**:
   - Confirm YAML frontmatter is valid
   - Check that description is comprehensive and includes triggers
   - Ensure examples are runnable
   - Verify references are properly linked if used

## Reference

See `references/skill-template.md` for the standard structure and `references/analysis-guide.md` for determining appropriate detail level.
