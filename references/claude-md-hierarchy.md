# CLAUDE.md Memory Hierarchy Reference
## Source: `src/utils/claudemd.ts`

## Memory File Loading Order (Priority: First = Lowest)

```typescript
// Memory file hierarchy (loaded in reverse priority order):
//
// 1. Project (.claude/rules/*.md) - Checked-in rules
// 2. Local (CLAUDE.local.md) - Private project-specific
// 3. User (~/.claude/CLAUDE.md) - Private global instructions
// 4. Managed (/etc/claude-code/CLAUDE.md) - Global instructions
//
// Actually loaded in: [Managed] + [User] + [Project] + [Local]
// (Last overrides first for same keys)
```

## Frontmatter Features

```yaml
---
# Conditional rules based on file paths
paths:
  - "src/**/*.ts"
  - "*.js"

# Skills to load
skills:
  - skill-name

# Hooks configuration
hooks:
  preToolUse:
    - name: "hook-name"
      description: "what it does"
---

# @include directive for including other files
@include ./common-rules.md
```

## Memory Types

```typescript
type MemoryType = {
  user: "Facts about the user/preferences"
  feedback: "User corrections"
  project: "Project context not derivable from code"
  reference: "External systems/links"
}
```

## Key Features

- `@include` directive for including other files
- Frontmatter `paths` for conditional rules (glob patterns)
- HTML comment stripping from markdown
- MEMORY_INSTRUCTION_PROMPT wrapper for context injection
