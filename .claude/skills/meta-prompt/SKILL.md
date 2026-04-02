---
name: meta-prompt
description: A systematic approach to designing professional Agent prompts. Based on Claude Code source code analysis.
when_to_use: When you need to design or write a professional agent prompt for any AI coding assistant
allowed_tools: ["Read", "Write", "Edit", "Glob", "Grep"]
agent: general-purpose
---

# Universal Agent Prompt Engineering Framework

This skill crafts elite-level prompts for AI coding agents based on **two pillars**:
1. **How to work with AI** — Foundational collaboration principles
2. **How to write agent prompts** — Technical implementation

---

# Part I: Foundational AI Collaboration Principles
## (Prerequisites for Writing Good Agent Prompts)

Before writing any agent prompt, you must understand how AI collaboration works. These principles inform every design decision.

---

## 1. The Mental Model: AI as a Smart Intern

Treat AI like a **smart but inexperienced colleague** — someone who:
- Needs **clear, specific instructions** (not vague goals)
- Benefits from **step-by-step guidance** (not "do everything at once")
- Needs **timely feedback** (correct mistakes immediately)
- Works best with **checkpoints** (confirm before proceeding)

```
❌ "Build a complete blog system"
✅ "First: Design the database schema. I'll review it. Then: Implement API routes. I'll review..."

❌ "Fix the bug"
✅ "1. First identify the root cause. 2. Propose 2-3 solutions. 3. I'll pick one. 4. Then implement."
```

---

## 2. Context is Everything

AI works entirely from context — **no context, no quality**. When designing prompts:

| Principle | Practice |
|-----------|----------|
| **Give background** | What is the project, tech stack, constraints |
| **Set constraints** | What MUST/NOT use, priorities |
| **Incremental delivery** | Don't dump everything at once |
| **Session continuity** | Keep related work in one conversation |

**Warning**: A prompt without context = generic AI behavior.

---

## 3. Task Decomposition (The Most Important Principle)

AI excels at **small, well-defined tasks**; fails at **vague, multi-part goals**.

```
❌ "Do X, Y, Z all at once"
✅ "Do X. Wait for confirmation. Then do Y. Wait for confirmation. Then do Z."
```

**When designing an agent**, build this decomposition INTO the prompt:

```
Your process:
1. [Specific step one]
2. [Confirm/Report step one]
3. [Specific step two]
4. [Confirm/Report step two]
...
```

---

## 4. Checkpoint Thinking

For critical operations, **never let AI run unsupervised**:

```
Guidelines:
- Before executing: Explain your approach
- After each step: Report findings/results
- If uncertain: Stop and ask
- I will review before you proceed to the next phase
```

This prevents wasted work when AI goes down wrong paths.

---

## 5. Chain of Constraint — Guide Thinking, Don't Just Give Tasks

Instead of "do X", **constrain HOW AI thinks**:

```
Analyze using this exact sequence:
1. List known conditions
2. Identify core contradictions
3. Evaluate exactly 3 approaches (no more, no less)
4. Recommend best option with explicit reasoning
DO NOT skip steps. DO NOT add steps.
```

**Key patterns:**

| Pattern | Example |
|---------|---------|
| **Step enumeration** | "In exactly this order: 1, 2, 3, 4" |
| **Output format** | "Format: ```section: content```" |
| **Example injection** | "Follow this exact format: ..." |
| **Error-first** | "This approach will fail because... Design around it" |
| **Reverse debugging** | "If you're wrong about X, where else could the bug be?" |

---

## 6. Constraint Over Description

Vague descriptions create unpredictable behavior:

```
❌ "Code should be clean and efficient"
✅ "Function max 15 lines. Variables max 3 words. Nesting max 2 levels. No magic numbers."

❌ "Don't use that approach, it's problematic"
✅ "Must NOT use Promise.all (rate limit issue). Use sequential requests with 100ms delays."
```

---

## 7. Session Management (For Long Conversations)

When designing prompts for ongoing tasks:

```
Periodically:
- Summarize completed work
- Re-state current focus
- Flag any deviations from original plan

Example:
"Based on our progress: [summary]. Current focus: [this].
Next: [that]. Any concerns: [ask]."
```

---

## 8. Failure Recovery

When AI makes mistakes, **immediate correction with context**:

```
❌ "That's wrong"
✅ "The error is [specific]. The actual value was [X], not [Y].
This means [interpretation]. Please re-analyze with this correction."
```

Design prompts to INCLUDE recovery mechanisms:

```
If your answer conflicts with [known fact/tool output],
stop and re-evaluate rather than doubling down.
```

---

## 9. Self-Debugging Inoculation

Counter AI's tendency to defend wrong answers:

```
If the bug is NOT in location A, where could it be?
List 3 possibilities you haven't considered.
```

```
Before giving your answer, check: "What could be wrong with this?"
```

---

## Summary: Foundation Principles Checklist

Before finalizing any agent prompt, verify:

- [ ] Does it treat AI as a smart intern needing guidance?
- [ ] Is context provided (background, constraints, scope)?
- [ ] Are tasks decomposed into clear steps?
- [ ] Are there checkpoints for critical operations?
- [ ] Does it guide thinking (not just assign tasks)?
- [ ] Are constraints specific and enforceable?
- [ ] Is there a failure recovery mechanism?
- [ ] Does output format match your needs?

---

# Part II: Technical Prompt Engineering
## (How to Write Agent Prompts)

Now apply the principles above into structured agent prompts.

---

## Core Prompt Anatomy (The 6 Sections)

Every professional agent prompt consists of these sections:

### Section 1: Identity Declaration

```
You are a [SPECIFIC ROLE] for [PLATFORM], [COMPANY]'s official [PRODUCT].
```

**Examples:**
- `You are a file search specialist for Claude Code, Anthropic's official CLI for Claude.`
- `You are a software architect and planning specialist for Claude Code.`
- `You are a verification specialist. Your job is not to confirm the implementation works — it's to try to break it.`

---

### Section 2: Critical Restrictions Block

```
=== CRITICAL: [MODE] - NO [ACTIONS] ===

This is a [MODE] task. You are STRICTLY PROHIBITED from:
- [Forbidden action 1]
- [Forbidden action 2]
...
```

**READ-ONLY agents:**
```
=== CRITICAL: READ-ONLY MODE - NO FILE MODIFICATIONS ===
- Creating new files (no Write, touch, or file creation of any kind)
- Modifying existing files (no Edit operations)
- Deleting files (no rm or deletion)
- Moving or copying files (no mv or cp)
- Creating temporary files anywhere, including /tmp
- Using redirect operators (>, >>, |) or heredocs to write to files
- Running ANY commands that change system state
```

**VERIFICATION agents:**
```
=== CRITICAL: DO NOT MODIFY THE PROJECT ===
You are STRICTLY PROHIBITED from:
- Creating, modifying, or deleting any files IN THE PROJECT DIRECTORY
- Installing dependencies or packages
- Running git write operations (add, commit, push)
```

---

### Section 3: Strengths (Core Competencies)

```
Your strengths:
- [Specific capability 1 - be concrete]
- [Specific capability 2]
- [Specific capability 3]
```

---

### Section 4: Operational Guidelines

```
Guidelines:
- Use [TOOL_A] for [SPECIFIC_TASK]
- Use [TOOL_B] for [SPECIFIC_TASK]
- NEVER use [TOOL_C] for: [LIST_OF_PROHIBITED_USES]
```

**Apply Foundation Principle #6 here**: Make constraints specific, not vague.

---

### Section 5: Task-Specific Strategy

**Verification Agent Pattern:**
```
=== VERIFICATION STRATEGY ===
Adapt based on what was changed:

**Frontend changes**: Start dev server → check browser automation → curl subresources → run frontend tests
**Backend/API changes**: Start server → curl/fetch endpoints → verify response shapes → test error handling
**CLI/script changes**: Run with inputs → verify stdout/stderr/exit codes → test edge cases
...
```

**Analysis Agent Pattern:**
```
=== ANALYSIS APPROACH ===
1. **Scope**: What exactly to analyze
2. **Search**: Where to look, what patterns
3. **Criteria**: How to judge findings
4. **Output**: Report structure
```

**Apply Foundation Principle #5 here**: Guide thinking with constrained steps.

---

### Section 6: Output Requirements

```
=== OUTPUT FORMAT (REQUIRED) ===
Every [action] MUST follow this structure:
[Structure specification]

End with exactly this line:
[REQUIRED_TERMINATION]
```

---

## Agent Definition Schema (Claude Code Specific)

```yaml
type AgentDefinition = {
  agentType: string            # Unique identifier
  whenToUse: string           # When to use (coordinator decisions)
  tools?: string[]            # Allowed tools whitelist
  disallowedTools?: string[]  # Forbidden tools blacklist
  skills?: string[]           # Preloaded skills
  model?: 'sonnet' | 'opus' | 'haiku' | 'inherit'
  effort?: 'low' | 'medium' | 'high' | 'max'
  permissionMode?: 'default' | 'dontAsk' | 'bypassPermissions'
  maxTurns?: number
  memory?: 'user' | 'project' | 'local'
  omitClaudeMd?: boolean     # Skip CLAUDE.md
  initialPrompt?: string      # Prepend before first user message
  background?: boolean        # Always run as background
  isolation?: 'worktree' | 'remote'
}
```

---

## Universal Tool Sets (Claude Code Reference)

**All Agents Disallowed:**
- TaskOutputTool, ExitPlanModeTool, EnterPlanModeTool
- AgentTool (except ant users), AskUserQuestionTool, TaskStopTool, WorkflowTool

**Async/Background Agent Allowed:**
- Read, WebSearch, TodoWrite, Grep, WebFetch, Glob
- Bash, Edit, Write, NotebookEdit, Skill
- SyntheticOutput, ToolSearch, EnterWorktree, ExitWorktree

---

## Cross-Platform Prompt Templates

### Template 1: Research/Analysis Agent
```
You are a [ROLE] for [PLATFORM].

=== CRITICAL: READ-ONLY MODE ===
You CANNOT write, edit, create, or delete any files.

Your strengths:
- [List 3-4 core competencies]

Guidelines:
- Use [SEARCH_TOOL] for [TASK]
- Use [READ_TOOL] when you know the specific file path
- NEVER use [WRITE_TOOL] for: [PROHIBITED_OPERATIONS]

Your task: [DETAILED_TASK_DESCRIPTION]

Process (APPLY FOUNDATION PRINCIPLES):
1. [Specific step one - concrete, not vague]
2. [Confirm/report findings]
3. [Step two]
...

End your response with:
### Critical Files
- [File path 1]
- [File path 2]
```

### Template 2: Implementation Agent
```
You are a [ROLE] for [PLATFORM].

Your task: [IMPLEMENTATION_TASK]

Guidelines (APPLY CONSTRAINTS):
- Use [TOOL] for [SPECIFIC_PURPOSE]
- Constraint: [SPECIFIC_RULE] (NOT vague descriptions)
- Prefer [EXISTING_PATTERN] over creating new code

Process (APPLY CHECKPOINT THINKING):
1. [Research/verify prerequisite]
2. [Implement specific change]
3. [Verify result]
4. Report concisely

If uncertain at any step: STOP and ask.
```

### Template 3: Verification Agent
```
You are a verification specialist. Your job is not to confirm
the implementation works — it's to try to break it.

=== CRITICAL: DO NOT MODIFY THE PROJECT ===
You MAY write ephemeral test scripts to /tmp but MUST clean up.

=== RECOGNIZE YOUR OWN RATIONALIZATIONS ===
(APPLY SELF-DEBUGGING INOCULATION)
- "The code looks correct" — RUN IT
- "Tests already pass" — VERIFY INDEPENDENTLY
- "Probably fine" — RUN IT

=== VERIFICATION STRATEGY ===
[Type-specific strategy]

=== REQUIRED STEPS ===
1. Read project conventions
2. Run build — broken build = FAIL
3. Run test suite — failing tests = FAIL
4. Run linters/type-checkers
5. Check for regressions

=== ADVERSARIAL PROBES ===
Try to break it:
- Concurrency issues
- Boundary values (0, -1, empty, MAX_INT)
- Idempotency (same request twice)
- Orphan operations

=== OUTPUT FORMAT ===
Every check MUST follow:
```
### Check: [what you're verifying]
**Command run:** [exact command]
**Output observed:** [actual output]
**Result:** PASS | FAIL
```

End with:
```
VERDICT: PASS | FAIL | PARTIAL
```
```

---

## Best Practices Checklist

### Foundation (From Part I):
- [ ] Treats AI as smart intern needing guidance
- [ ] Provides sufficient context
- [ ] Tasks are decomposed into steps
- [ ] Checkpoints for critical operations
- [ ] Thinking path is constrained
- [ ] Constraints are specific (not vague)
- [ ] Has failure recovery mechanism
- [ ] Self-debugging inoculation included

### Technical (From Part II):
- [ ] Clear identity declaration
- [ ] `=== CRITICAL: [MODE] ===` for restrictions
- [ ] Specific tool guidance (exact names)
- [ ] Prohibited actions listed explicitly
- [ ] Task-specific strategy included
- [ ] Output format specified
- [ ] VERDICT format for verification agents

### DON'T:
- [ ] Vague role names ("helpful assistant")
- [ ] "Based on your findings" without specifying findings
- [ ] Generic tool guidance
- [ ] Skipping the critical restrictions block
- [ ] Delegating synthesis ("fix based on research" without findings)
- [ ] Same structure for every agent type
- [ ] Forgetting platform-specific conventions

---

## Platform-Specific Notes

### Claude Code
- Tool names: `FILE_READ_TOOL_NAME`, `BASH_TOOL_NAME`
- Embedded tools (Ant): use `find`/`grep` via Bash instead of Glob/Grep
- `omitClaudeMd: true` skips CLAUDE.md
- `model: 'inherit'` uses parent model
- Fork (no `subagent_type`) inherits full context

### GitHub Codex
- OpenAI tool calling format
- Different tool schema structure
- Different context window constraints

### OpenClaw
- Check specific tool naming
- Model capability restrictions vary

---

## Usage Examples

### Example 1: Security Review Agent
```
/meta-prompt

Create a code review agent:
- Role: Security vulnerability researcher
- Platform: Claude Code
- Read-only analysis
- Focus: injection, auth bypass, data exposure
- Include adversarial thinking patterns
```

### Example 2: API Documentation Agent
```
/meta-prompt

Create a documentation agent:
- Reads code, generates API docs
- OpenAPI/Swagger conventions
- Markdown output
- Step-by-step: identify endpoints → extract params → generate spec
```

---

## Memory Anchor

Based on patterns from Claude Code internals:
- `src/tools/AgentTool/built-in/exploreAgent.ts`
- `src/tools/AgentTool/built-in/planAgent.ts`
- `src/tools/AgentTool/built-in/verificationAgent.ts`
- `src/tools/AgentTool/prompt.ts`
- `src/constants/tools.ts`
- `src/tools/AgentTool/loadAgentsDir.ts`

And AI collaboration principles synthesized from practical experience.
