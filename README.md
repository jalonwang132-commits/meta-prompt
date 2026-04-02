# meta-prompt

> Not just templates. A systematic approach to designing professional Agent prompts.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai)

**A prompt engineering framework based on Claude Code's source code analysis.**

---

## The Problem

Most prompt writing is guesswork:

```
❌ "You are a code reviewer, check my code for bugs"
```

→ AI does whatever it wants
→ Output is unpredictable
→ Results vary wildly between AI models

## The Solution

After analyzing Claude Code's built-in Agent prompts, I discovered a **6-section structure** that produces consistent, professional results:

```
1. Identity Declaration
2. Critical Restrictions Block
3. Strengths Section
4. Operational Guidelines
5. Task-Specific Strategy
6. Output Requirements
```

---

## Why This Works

| Principle | How It Helps |
|-----------|--------------|
| **Constraints > Descriptions** | Specific rules prevent AI going off-track |
| **Checkpoint Thinking** | AI confirms before proceeding, no wasted work |
| **Adversarial Design** | Built-in self-critique prevents AI from defending wrong answers |
| **Tool Configuration** | Explicit tool definitions, no guessing |

---

## Quick Start

### For Claude Code Users

**Installation:**
```bash
# Copy SKILL.md to your Claude Code skills directory
mkdir -p ~/.claude/skills/meta-prompt
cp SKILL.md ~/.claude/skills/meta-prompt/
```

**Usage:**
```
/meta-prompt

Create a code review agent that focuses on security vulnerabilities.
```

### Structure Overview

```
meta-prompt/
├── SKILL.md                          # Claude Code skill (install this)
├── README.md                         # This file
├── references/                       # Source research
│   ├── agent-schema.md              # Agent definition types
│   ├── tool-restrictions.md          # Tool filtering rules
│   ├── built-in-agents.md           # Explore/Plan/Verification prompts
│   └── prompt-writing-guidelines.md  # Writing guidelines
└── .claude/
    └── skills/
        └── meta-prompt/
            └── SKILL.md              # Direct installation path
```

---

## The 6-Section Framework

### 1. Identity Declaration

```markdown
You are a [SPECIFIC ROLE] for [PLATFORM], [COMPANY]'s official [PRODUCT].
```

### 2. Critical Restrictions Block

```markdown
=== CRITICAL: [MODE] - NO [ACTIONS] ===

This is a [MODE] task. You are STRICTLY PROHIBITED from:
- [Forbidden action 1]
- [Forbidden action 2]
...
```

### 3. Strengths Section

```markdown
Your strengths:
- [Specific capability 1]
- [Specific capability 2]
- [Specific capability 3]
```

### 4. Operational Guidelines

```markdown
Guidelines:
- Use [TOOL] for [SPECIFIC_TASK]
- NEVER use [TOOL] for: [PROHIBITED_USES]
```

### 5. Task-Specific Strategy

```markdown
=== VERIFICATION STRATEGY ===
Adapt based on what was changed:
- Frontend: Start dev server → check automation tools → curl resources → run tests
- Backend: Start server → curl endpoints → verify shapes → test edge cases
```

### 6. Output Requirements

```markdown
=== OUTPUT FORMAT (REQUIRED) ===
Every check MUST follow this structure:
[Structure specification]

End with exactly:
VERDICT: PASS | FAIL | PARTIAL
```

---

## Comparison

| Aspect | Without Structure | With meta-prompt |
|--------|-------------------|------------------|
| **Output** | Unpredictable | Consistent |
| **Tools** | AI guesses | Explicitly defined |
| **Constraints** | Vague | Specific & enforced |
| **Failure Recovery** | None | Built-in checkpoints |
| **Cross-AI** | Results vary | Results stable |

---

## AI Collaboration Principles

Before writing prompts, understand how AI works:

- **Treat AI as a smart intern** — needs clear instructions, step-by-step guidance
- **Context is everything** — no context = generic behavior
- **Task decomposition** — small, clear tasks > vague, complex goals
- **Checkpoint thinking** — critical operations need human review
- **Chain of constraint** — guide thinking, don't just assign tasks
- **Constraint over description** — specific rules > vague requirements

See [SKILL.md](SKILL.md) for the complete framework.

---

## Resources

- [Complete Skill Documentation](SKILL.md)
- [Agent Schema Reference](references/agent-schema.md)
- [Tool Restrictions Reference](references/tool-restrictions.md)
- [Built-in Agent Prompts](references/built-in-agents.md)
- [Prompt Writing Guidelines](references/prompt-writing-guidelines.md)

---

## Background

This project emerged from analyzing Claude Code's internal source code to understand how its built-in Agents (Explore, Plan, Verification) are designed.

The findings:
1. All built-in prompts follow a consistent 6-section structure
2. Specific constraint definitions prevent AI misalignment
3. Built-in checkpoint mechanisms prevent wasted work
4. Adversarial thinking patterns catch AI's tendency to defend wrong answers

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

MIT License - see [LICENSE](LICENSE)

---

*"The best AI output comes not from clever prompting, but from systematic prompt design."*
