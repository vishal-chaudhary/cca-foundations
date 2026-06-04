# Claude Certified Architect – Foundations (CCA-F)
### 7-Week Exam Prep Guide

A structured, week-by-week study plan for the **Claude Certified Architect – Foundations** certification exam.  
Built for solutions architects with strong system design backgrounds and zero LLM API experience.

---

## Study Plan Overview

| Week | Focus | Domain Weight | Hours |
|------|-------|--------------|-------|
| [Week 1](Week1-Foundations.md) | Foundations — Claude API, LLM Basics & Environment Setup | — | 6–8h |
| [Week 2](Week2-Agentic-Architecture.md) | Domain 1 — Agentic Architecture & Orchestration | 27% | 6–8h |
| [Week 3](Week3-Tool-Design-MCP.md) | Domain 2 — Tool Design & MCP Integration | 18% | 6–8h |
| [Week 4](Week4-Claude-Code-Configuration.md) | Domain 3 — Claude Code Configuration & Workflows | 20% | 6–8h |
| [Week 5](Week5-Prompt-Engineering-Structured-Output.md) | Domain 4 — Prompt Engineering & Structured Output | 20% | 6–8h |
| [Week 6](Week6-Context-Management-Reliability.md) | Domain 5 — Context Management & Reliability | 15% | 6–8h |
| [Week 7](Week7-Review-Exam-Simulation.md) | Full Review, Practice Questions & Exam Simulation | — | 6–8h |

**Total:** ~49–56 hours over 7 weeks

---

## Exam Domain Breakdown

```
Domain 1 — Agentic Architecture & Orchestration   ████████████████████████████ 27%
Domain 3 — Claude Code Configuration & Workflows  ████████████████████ 20%
Domain 4 — Prompt Engineering & Structured Output ████████████████████ 20%
Domain 2 — Tool Design & MCP Integration          ██████████████████ 18%
Domain 5 — Context Management & Reliability       ███████████████ 15%
```

---

## Must-Know Concepts (Quick Reference)

| Concept | What You Must Know |
|---------|--------------------|
| `stop_reason: tool_use` | Continue the loop — execute tool and append result |
| `stop_reason: end_turn` | Done — terminate the loop |
| `tool_choice: "auto"` | Model MAY not call a tool |
| `tool_choice: "any"` | Model MUST call a tool |
| `allowedTools` | Must include `"Task"` for coordinator to spawn subagents |
| `.mcp.json` | Project-scoped; shared via version control |
| `~/.claude.json` | User-scoped personal/experimental servers |
| CLAUDE.md hierarchy | User → Project → Directory (project is version controlled) |
| `context: fork` | Skill runs in isolated context — won't pollute main conversation |
| `-p` flag | Non-interactive mode for CI — prevents hanging |
| Batch API | 50% savings, up to 24h, NO latency SLA, NO multi-turn tool calling |
| Hooks vs. prompts | Hooks = deterministic; prompts = probabilistic |
| Subagent context | Never inherited — always explicitly injected |
| `isRetryable` | Transient = true; business/validation/permission = false |

---

## Priority Courses

### 🔴 Official Anthropic Courses (Do These First)

- [Building with the Claude API](https://www.coursera.org/learn/building-with-the-claude-api) — Coursera
- [Claude Code in Action](https://www.coursera.org/learn/claude-code-in-action) — Coursera
- [Introduction to Model Context Protocol](https://www.coursera.org/learn/introduction-to-model-context-protocol) — Coursera
- [Model Context Protocol: Advanced Topics](https://www.coursera.org/learn/model-context-protocol-advanced-topics) — Coursera

### 🟡 Free Resources

- [Anthropic Skilljar Platform](https://anthropic.skilljar.com) — Free official courses
- [Anthropic Certifications Practice Site](https://www.anthropiccertifications.com/courses) — 25 practice questions for CCA-F
- [Anthropic Official Docs](https://docs.anthropic.com) — Primary reference

---

## Week-by-Week Files

- [Week 1 — Foundations](Week1-Foundations.md)
- [Week 2 — Agentic Architecture & Orchestration](Week2-Agentic-Architecture.md)
- [Week 3 — Tool Design & MCP Integration](Week3-Tool-Design-MCP.md)
- [Week 4 — Claude Code Configuration & Workflows](Week4-Claude-Code-Configuration.md)
- [Week 5 — Prompt Engineering & Structured Output](Week5-Prompt-Engineering-Structured-Output.md)
- [Week 6 — Context Management & Reliability](Week6-Context-Management-Reliability.md)
- [Week 7 — Review & Exam Simulation](Week7-Review-Exam-Simulation.md)

---

## How to Use This Guide

1. Start with Week 1 even if you already have API experience — it sets up the environment for all hands-on exercises
2. Each week file has: concepts, course links, a hands-on exercise, and a checkbox to-do list
3. The to-do checklists are designed to be worked through sequentially — don't skip the hands-on exercises
4. Week 7 has no new courses — it's pure consolidation and practice questions

---

*Target score: 720+ | Recommended timeline: 7 weeks at 6–8 hours/week*
