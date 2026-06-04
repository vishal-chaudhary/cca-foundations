---
layout: default
title: Week 3 — Tool Design & MCP Integration
description: Domain 2 — 18% of the exam
---

[← Back to Study Plan](./)

# Week 3 — Domain 2: Tool Design & MCP Integration (18%)

**Duration:** 6–8 hours  
**Goal:** Design well-structured MCP tools with clear descriptions, proper error handling, and correct server configuration. Your API design experience is your biggest asset here.

---

## Why This Week Matters

MCP tools are just well-designed API contracts — your API design background transfers directly. The exam tests whether you know the *Claude-specific* rules: how tool descriptions drive selection, how errors must be structured for agent recovery, and how MCP server config is split across project vs. user scope.

---

## Core Concepts

### Tool Descriptions (Task 2.1)
- Descriptions are the **primary mechanism for tool selection** — treat them like API docs
- Include: purpose, input formats, example queries, edge cases, **when NOT to use this tool**
- Ambiguous/overlapping descriptions = misrouting (common exam distractor)
- System prompt wording can override tool descriptions (keyword sensitivity)

### Structured Error Responses (Task 2.2)
- `isError` flag in MCP responses
- Four error categories: **transient, validation, business, permission**
- `isRetryable` boolean — critical for agent recovery decisions
  - Transient errors → `isRetryable: true`
  - Business/validation/permission errors → `isRetryable: false`
- Never return generic "Operation failed" — it prevents intelligent recovery

### Tool Distribution (Task 2.3)
- Too many tools (e.g., 18 vs. 4–5) degrades selection reliability
- Scope tools to agent role: search agent gets search tools only
- `tool_choice` options:
  - `"auto"` = model may or may not call a tool
  - `"any"` = model **must** call a tool (prevents conversational text response)
  - `{"type": "tool", "name": "..."}` = specific tool forced

### MCP Server Configuration (Task 2.4)
| Scope | Location | Use Case |
|-------|----------|----------|
| Project-level | `.mcp.json` in project root | Shared via version control, team tooling |
| User-level | `~/.claude.json` | Personal/experimental servers |

- Environment variable expansion: `${GITHUB_TOKEN}` for secrets
- **Never commit credentials** — always use `${ENV_VAR}` expansion
- MCP resources: expose content catalogs to reduce exploratory tool calls

### Built-in Tools (Task 2.5)
| Tool | Use For |
|------|---------|
| `Grep` | Search file **contents** (function names, error messages, imports) |
| `Glob` | Match file **paths** by pattern (`**/*.test.tsx`) |
| `Edit` | Targeted modification using unique text anchor |
| `Read` + `Write` | Fallback when Edit fails due to non-unique text |

Incremental exploration strategy: Grep for entry points → Read to follow imports

---

## Courses

### 🔴 Priority 1 — Anthropic Official

| Course | Platform | Link |
|--------|----------|------|
| Introduction to Model Context Protocol | Coursera (Anthropic) | [coursera.org/learn/introduction-to-model-context-protocol](https://www.coursera.org/learn/introduction-to-model-context-protocol) |
| Model Context Protocol: Advanced Topics | Coursera (Anthropic) | [coursera.org/learn/model-context-protocol-advanced-topics](https://www.coursera.org/learn/model-context-protocol-advanced-topics) |

> Do these in order. The intro course builds your first MCP server from scratch. The advanced course covers production patterns that appear in the exam's harder questions.

### 🟡 Priority 2 — Coursera Supplemental

| Course | Link | Why |
|--------|------|-----|
| AI Agents with MCP Specialization (Vanderbilt) | [coursera.org/specializations/ai-agents-model-context-protocol](https://www.coursera.org/specializations/ai-agents-model-context-protocol) | Error recovery systems map directly to Task 2.2 |

### 🟢 Free Resources

| Resource | Link |
|----------|------|
| Official MCP Introduction | [docs.anthropic.com/en/docs/build-with-claude/mcp](https://docs.anthropic.com/en/docs/build-with-claude/mcp) |
| MCP Quickstart | [modelcontextprotocol.io/quickstart](https://modelcontextprotocol.io/quickstart) |
| MCP Specification — Tools | [modelcontextprotocol.io/docs/concepts/tools](https://modelcontextprotocol.io/docs/concepts/tools) |
| Tool Use Best Practices | [docs.anthropic.com/en/docs/build-with-claude/tool-use/best-practices-for-tool-definitions](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/best-practices-for-tool-definitions) |
| MCP Example Servers (GitHub) | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) |
| Anthropic Skilljar — Introduction to MCP | [anthropic.skilljar.com/introduction-to-model-context-protocol](https://anthropic.skilljar.com/introduction-to-model-context-protocol) |
| Fireship — Model Context Protocol Explained | [youtube.com/watch?v=7j_NE6Pjv-E](https://www.youtube.com/watch?v=7j_NE6Pjv-E) |

### 🔵 Pluralsight (Supplemental)

| Course | Link |
|--------|------|
| Evaluating and Optimizing LLM Agents | [pluralsight.com/courses/evaluating-optimizing-llm-agents](https://www.pluralsight.com/courses/evaluating-optimizing-llm-agents) |

Focus on tool selection reliability and agent evaluation sections (Task 2.3).

---

## Hands-On Exercise

Build a simple MCP server with 2 tools that have **similar but distinct functionality**:

1. Define both tools with clear, differentiated descriptions (e.g., `search_products` vs. `lookup_product_by_id`)
2. Write descriptions that explicitly state when NOT to use each tool
3. Add structured error responses with `isError`, `errorCategory`, and `isRetryable` fields
4. Test with ambiguous requests — verify the correct tool is selected
5. Configure the server in `.mcp.json` using `${ENV_VAR}` for any credentials

Example error response structure:
```json
{
  "isError": true,
  "errorCategory": "transient",
  "isRetryable": true,
  "message": "Database connection timeout — retry after 2 seconds",
  "attemptedQuery": "SELECT * FROM orders WHERE id = 12345"
}
```

---

## Exam Watch-Outs

| Wrong Answer Pattern | Correct Approach |
|----------------------|-----------------|
| Generic "Operation failed" error | Structured error with category + isRetryable |
| Commit API keys in `.mcp.json` | Use `${ENV_VAR}` expansion |
| Give all agents all tools | Scope tools to agent role |
| `tool_choice: "auto"` when tool is required | Use `tool_choice: "any"` |
| Overlapping tool descriptions | Each description must state when NOT to use it |

---

## To-Do List

- [ ] Read [MCP official introduction](https://docs.anthropic.com/en/docs/build-with-claude/mcp)
- [ ] Read [Tool Use Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/best-practices-for-tool-definitions)
- [ ] Read [MCP Tools Specification](https://modelcontextprotocol.io/docs/concepts/tools)
- [ ] Browse [MCP Example Servers](https://github.com/modelcontextprotocol/servers) — study tool definition patterns
- [ ] Complete Coursera: **Introduction to Model Context Protocol** (Anthropic)
- [ ] Complete Coursera: **Model Context Protocol: Advanced Topics** (Anthropic)
- [ ] Watch Fireship — Model Context Protocol Explained (~8 min)
- [ ] Build the MCP server exercise:
  - [ ] Define 2 tools with similar but distinct purposes
  - [ ] Write descriptions with explicit "when NOT to use" guidance
  - [ ] Implement structured error responses with `isError`, `errorCategory`, `isRetryable`
  - [ ] Configure in `.mcp.json` with env var for credentials
  - [ ] Test tool selection with ambiguous queries
- [ ] Memorize the 4 error categories: transient, validation, business, permission
- [ ] Know which categories are `isRetryable: true` vs. `false`
- [ ] Know the difference between `.mcp.json` (project) and `~/.claude.json` (user)
