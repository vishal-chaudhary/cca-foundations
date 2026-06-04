# Week 2 — Domain 1: Agentic Architecture & Orchestration (27%)

**Duration:** 6–8 hours  
**Goal:** Master the agentic loop, coordinator-subagent patterns, and multi-agent orchestration. This is the heaviest exam domain — don't rush it.

---

## Why This Week Matters

Domain 1 is 27% of the exam — more than any other domain. Your orchestration/microservices background is a genuine advantage: coordinator-subagent patterns map directly to hub-and-spoke service architectures, and agentic loops are just event-driven state machines. The gap is the Claude-specific vocabulary and the specific failure modes the exam tests.

---

## Core Concepts

### Agentic Loops (Task 1.1)
The fundamental loop:
1. Send request → check `stop_reason`
2. If `tool_use` → execute the tool → append result to `messages` → repeat
3. If `end_turn` → done

**Anti-patterns to know:**
- Parsing natural language to detect completion (wrong — use `stop_reason`)
- Arbitrary iteration caps without structured termination conditions

### Multi-Agent Coordinator-Subagent (Task 1.2)
- **Hub-and-spoke**: coordinator owns all routing, error handling, result aggregation
- Subagents have **isolated context** — they do NOT inherit parent history
- Narrow task decomposition is a failure mode (exam question: "only visual arts" scope)

### Subagent Spawning & Context Passing (Task 1.3)
- `Task` tool is the spawning mechanism
- `allowedTools` must include `"Task"` for coordinator to spawn subagents
- Context must be **explicitly injected** into each subagent prompt
- Parallel spawning: emit multiple `Task` calls in one coordinator response

### Workflow Enforcement (Task 1.4)
- **Programmatic prerequisites beat prompt instructions** for critical sequences
- Hooks (`PostToolUse`) for tool call interception
- Structured handoff summaries when escalating to humans

### SDK Hooks (Task 1.5)
- `PostToolUse`: normalize heterogeneous data formats from different MCP tools
- Tool call interception: block policy violations (e.g., refunds > $500)
- **Hooks = deterministic; prompts = probabilistic** — remember this for the exam

### Task Decomposition (Task 1.6)
- Prompt chaining: sequential, predictable workflows
- Dynamic decomposition: open-ended tasks where next steps depend on discoveries

### Session Management (Task 1.7)
- `--resume <session-name>` to continue named sessions
- `fork_session` for parallel exploration branches from shared baseline
- Know when to resume vs. start fresh with injected summary

---

## Courses

### 🔴 Priority 1 — Anthropic Official

| Course | Platform | Scope This Week |
|--------|----------|-----------------|
| [Building with the Claude API](https://www.coursera.org/learn/building-with-the-claude-api) | Coursera (Anthropic) | **Modules 5–7** — Agentic workflows, tool use, orchestration |

### 🟡 Priority 2 — Coursera Supplemental

| Course | Link | Why |
|--------|------|-----|
| AI Agents with MCP Specialization (Vanderbilt) | [coursera.org/specializations/ai-agents-model-context-protocol](https://www.coursera.org/specializations/ai-agents-model-context-protocol) | "Universal agent loop" + Failing Forward patterns map directly to Domain 1 |

### 🔵 Pluralsight (Primary for this domain)

| Course | Link | Focus |
|--------|------|-------|
| Introduction to Developing AI Agents | [pluralsight.com/courses/introduction-developing-ai-agents](https://www.pluralsight.com/courses/introduction-developing-ai-agents) | Agent architecture, tool use, reasoning loop |
| Applying Multi-Agent Systems to Daily Tasks | [pluralsight.com/courses/applying-multi-agent-systems-daily-tasks](https://www.pluralsight.com/courses/applying-multi-agent-systems-daily-tasks) | Hierarchical/coordinator-subagent patterns |
| Building Multi-Agent Systems with AutoGen | [pluralsight.com/courses/building-multi-agent-systems-autogen](https://www.pluralsight.com/courses/building-multi-agent-systems-autogen) | Orchestration patterns that transfer to Claude Agent SDK |
| Agentic AI Safety and Alignment | [pluralsight.com/courses/agentic-ai-safety-alignment](https://www.pluralsight.com/courses/agentic-ai-safety-alignment) | Escalation, enforcement, guardrails |

Full path: [Agentic AI for Developers](https://www.pluralsight.com/paths/agentic-llms-for-developers)

### 🟢 Free Resources

| Resource | Link |
|----------|------|
| Tool Use Overview (official) | [docs.anthropic.com/en/docs/build-with-claude/tool-use/overview](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/overview) |
| Implementing Tool Use | [docs.anthropic.com/en/docs/build-with-claude/tool-use/implementing-tool-use](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/implementing-tool-use) |
| Agents Overview (official) | [docs.anthropic.com/en/docs/build-with-claude/agents-and-tools/agents-overview](https://docs.anthropic.com/en/docs/build-with-claude/agents-and-tools/agents-overview) |
| Claude Agent SDK | [docs.anthropic.com/en/docs/agents-and-tools/claude-code/sdk](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/sdk) |
| DeepLearning.AI — AI Agents in LangGraph | [deeplearning.ai/short-courses/ai-agents-in-langgraph/](https://www.deeplearning.ai/short-courses/ai-agents-in-langgraph/) |
| DeepLearning.AI — Multi AI Agent Systems with crewAI | [deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai/](https://www.deeplearning.ai/short-courses/multi-ai-agent-systems-with-crewai/) |

---

## Hands-On Exercise

Build a Customer Support agent (mirrors an actual exam scenario):

1. Define 3 tools: `get_customer`, `lookup_order`, `process_refund`
2. Implement the full agentic loop checking `stop_reason`
3. Add a **programmatic prerequisite**: block `process_refund` until `get_customer` returns a verified customer ID
4. Add a `PostToolUse` hook that **blocks refunds over $500** and returns a structured error

This single exercise covers Tasks 1.1, 1.4, and 1.5.

---

## Exam Watch-Outs

| Wrong Answer Pattern | Correct Approach |
|----------------------|-----------------|
| Parse text output to detect completion | Check `stop_reason` field |
| Use prompt instructions to enforce refund limits | Use a `PostToolUse` hook (deterministic) |
| Subagents inherit coordinator context | Context must be explicitly injected |
| `allowedTools` doesn't include `"Task"` | Coordinator can't spawn subagents |
| Fix downstream subagent when routing is wrong | Fix the coordinator's decomposition |

---

## To-Do List

- [ ] Read [Agents Overview](https://docs.anthropic.com/en/docs/build-with-claude/agents-and-tools/agents-overview) (official docs)
- [ ] Read [Tool Use Overview](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/overview) (official docs)
- [ ] Read [Implementing Tool Use](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/implementing-tool-use)
- [ ] Complete Coursera: Building with the Claude API — **Modules 5–7**
- [ ] Complete Pluralsight: Introduction to Developing AI Agents
- [ ] Complete Pluralsight: Applying Multi-Agent Systems to Daily Tasks
- [ ] Complete Pluralsight: Building Multi-Agent Systems with AutoGen
- [ ] Complete Pluralsight: Agentic AI Safety and Alignment
- [ ] Watch DeepLearning.AI: AI Agents in LangGraph (free)
- [ ] Build the Customer Support agent exercise
  - [ ] Define 3 tools with JSON schemas
  - [ ] Implement the agentic loop with `stop_reason` checking
  - [ ] Add programmatic prerequisite blocking `process_refund`
  - [ ] Add `PostToolUse` hook blocking refunds > $500
- [ ] Write in your own words: what is the difference between hooks and prompt instructions?
- [ ] Confirm you know: what must `allowedTools` contain for a coordinator to spawn subagents?
