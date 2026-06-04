---
layout: default
title: Week 4 — Claude Code Configuration & Workflows
description: Domain 3 — 20% of the exam
---

[← Back to Study Plan](./)

# Week 4 — Domain 3: Claude Code Configuration & Workflows (20%)

**Duration:** 6–8 hours  
**Goal:** Learn Claude Code configuration hierarchy, custom commands, skills, and CI/CD integration. This is the most Claude-specific domain — no prior analogy, learn it directly.

---

## Why This Week Matters

Domain 3 is 20% of the exam and is entirely about Claude Code tooling. Unlike Domains 1–2 which map to familiar architecture patterns, this domain requires learning Claude-specific configuration systems from scratch. The single Anthropic Coursera course this week covers virtually all of it.

---

## Core Concepts

### CLAUDE.md Hierarchy (Task 3.1)

| Level | Location | Scope |
|-------|----------|-------|
| User | `~/.claude/CLAUDE.md` | Personal only, NOT version controlled |
| Project | `.claude/CLAUDE.md` or root `CLAUDE.md` | Shared with team via version control |
| Directory | `subdirectory/CLAUDE.md` | Scoped to that directory only |

- `@import` syntax for modular organization
- `.claude/rules/` directory for topic-specific rule files
- **Project-level is version controlled** — key exam fact

### Custom Slash Commands & Skills (Task 3.2)

| Location | Scope |
|----------|-------|
| `.claude/commands/` | Project-scoped, version controlled, team-shared |
| `~/.claude/commands/` | User-scoped, personal only |

Skill frontmatter options in `.claude/skills/` with `SKILL.md`:
- `context: fork` — isolated sub-agent context (prevents polluting main conversation)
- `allowed-tools` — restricts tool access during skill execution
- `argument-hint` — prompts user for parameters

> **Key distinction**: Skills = on-demand invocation; CLAUDE.md = always-loaded context

### Path-Specific Rules (Task 3.3)
- `.claude/rules/` files with YAML frontmatter `paths:` glob patterns
- Load **only** when editing matching files
- Reduces irrelevant context and token usage
- Better than subdirectory CLAUDE.md when conventions span multiple directories

### Plan Mode vs. Direct Execution (Task 3.4)
Use **Plan Mode** for:
- Complex tasks, large-scale changes
- Multiple valid approaches
- Architectural decisions
- Multi-file modifications

Use **Direct Execution** for:
- Simple, well-scoped, single-file changes with clear requirements

Use **Explore subagent** for:
- Isolating verbose discovery output
- Preserving main context cleanliness

### Iterative Refinement (Task 3.5)
- Concrete input/output examples beat prose descriptions
- Test-driven iteration: write tests first, share failures to guide improvement
- Interview pattern: let Claude ask questions before implementing
- Interacting problems → single message; independent problems → sequential

### CI/CD Integration (Task 3.6)
| Flag/Option | Purpose |
|-------------|---------|
| `-p` / `--print` | Non-interactive mode — **critical for CI, prevents hangs** |
| `--output-format json` | Machine-parseable output for PR comments |
| `--json-schema` | Structured output with schema validation |

- CLAUDE.md provides project context to CI-invoked Claude Code
- Independent review instance is more effective than self-review (no prior reasoning context)

---

## Courses

### 🔴 Priority 1 — Anthropic Official (⭐ Do Not Skip)

| Course | Platform | Link |
|--------|----------|------|
| Claude Code in Action | Coursera (Anthropic) | [coursera.org/learn/claude-code-in-action](https://www.coursera.org/learn/claude-code-in-action) |

> This single course covers essentially **all of Domain 3**. Make it your primary focus this week.

Covers: core tools, context management, `/init`, CLAUDE.md files, `@` mentions, Plan Mode, Thinking Mode, custom commands, MCP server integration, GitHub integration for automated PR reviews.

### 🟡 Priority 2 — Coursera Supplemental

| Course | Link |
|--------|------|
| Mastering Claude AI Specialization (Edureka) | [coursera.org/specializations/mastering-claude-ai-prompting-apis-rag-and-mcp](https://www.coursera.org/specializations/mastering-claude-ai-prompting-apis-rag-and-mcp) |

Use the Claude Code and workflow modules from this specialization as supplemental reading.

### 🟢 Free Resources

| Resource | Link |
|----------|------|
| Claude Code Overview (official) | [docs.anthropic.com/en/docs/claude-code/overview](https://docs.anthropic.com/en/docs/claude-code/overview) |
| Claude Code Quickstart | [docs.anthropic.com/en/docs/claude-code/quickstart](https://docs.anthropic.com/en/docs/claude-code/quickstart) |
| CLAUDE.md Configuration Guide | [docs.anthropic.com/en/docs/claude-code/memory](https://docs.anthropic.com/en/docs/claude-code/memory) |
| Claude Code in CI/CD (GitHub Actions) | [docs.anthropic.com/en/docs/claude-code/github-actions](https://docs.anthropic.com/en/docs/claude-code/github-actions) |
| Anthropic Skilljar — Claude Code in Action | [anthropic.skilljar.com/claude-code-in-action](https://anthropic.skilljar.com/claude-code-in-action) |

### 🔵 Pluralsight (Supplemental)

| Course | Link |
|--------|------|
| Agentic AI for Developers | [pluralsight.com/courses/agentic-ai-developers](https://www.pluralsight.com/courses/agentic-ai-developers) |
| Integrating Agentic AI for Developers (Path) | [pluralsight.com/paths/integrating-agentic-ai-for-developers](https://www.pluralsight.com/paths/integrating-agentic-ai-for-developers) |

---

## Hands-On Exercise

Install Claude Code first:
```bash
npm install -g @anthropic-ai/claude-code
```

Then build the following in a test project:

1. Create a **3-level CLAUDE.md hierarchy**:
   - `~/.claude/CLAUDE.md` with personal preferences
   - Root `CLAUDE.md` with project-wide instructions
   - `src/CLAUDE.md` with directory-specific rules

2. Add a `.claude/rules/` file scoped to `**/*.test.*` files with testing conventions

3. Create a **project-scoped slash command** in `.claude/commands/` (e.g., `/review`)

4. Build a **skill** with:
   - `context: fork`
   - `allowed-tools: [Read, Grep]`
   - An `argument-hint`

5. Run Claude Code with `-p` flag and capture JSON output:
   ```bash
   claude -p "List all TypeScript files in src/" --output-format json
   ```

---

## Exam Watch-Outs

| Wrong Answer Pattern | Correct Approach |
|----------------------|-----------------|
| Put personal config in project CLAUDE.md | Personal config goes in `~/.claude/CLAUDE.md` |
| Use project commands for personal workflows | Personal commands go in `~/.claude/commands/` |
| Run Claude in CI without `-p` flag | Always use `-p` to prevent interactive hangs |
| Use `context: fork` when sharing state is needed | `context: fork` = isolated — use only when isolation is desired |
| Have Claude review its own output | Use independent instance (no prior context) |

---

## To-Do List

- [ ] Install Claude Code: `npm install -g @anthropic-ai/claude-code`
- [ ] Read [Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- [ ] Read [CLAUDE.md Configuration Guide](https://docs.anthropic.com/en/docs/claude-code/memory)
- [ ] Read [Claude Code in CI/CD](https://docs.anthropic.com/en/docs/claude-code/github-actions)
- [ ] Complete Coursera: **Claude Code in Action** (Anthropic) — full course
- [ ] Create a test project with 3-level CLAUDE.md hierarchy
- [ ] Add a `.claude/rules/` file with a glob-scoped rule
- [ ] Create a project-scoped slash command in `.claude/commands/`
- [ ] Build a skill with `context: fork` and `allowed-tools`
- [ ] Run Claude Code with `-p` flag and observe JSON output
- [ ] Know the CLAUDE.md hierarchy by heart: user → project → directory
- [ ] Know which level is version controlled (project) vs. personal (user)
- [ ] Know when to use Plan Mode vs. direct execution
- [ ] Understand why `-p` is critical for CI/CD usage
