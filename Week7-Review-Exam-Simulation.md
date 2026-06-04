# Week 7 — Review, Practice Questions & Exam Simulation

**Duration:** 6–8 hours  
**Goal:** Consolidate everything, identify and close remaining gaps, simulate exam conditions, and walk in confident.

---

## No New Courses This Week

This week is entirely consolidation. Resist the urge to start new content. The exam is scenario-based — you need pattern recognition from review, not new information from new courses.

---

## Day-by-Day Plan

### Days 1–2: Domain Review
Go back through any domain where you felt shaky. Re-read the relevant task statements in the exam guide directly.

Priority order (by exam weight):
1. **Domain 1** — Agentic Architecture & Orchestration (27%) — highest weight, review first
2. **Domain 3** — Claude Code Configuration (20%)
3. **Domain 4** — Prompt Engineering & Structured Output (20%)
4. **Domain 2** — Tool Design & MCP Integration (18%)
5. **Domain 5** — Context Management & Reliability (15%)

On Coursera: re-watch any modules where you felt uncertain. The **Building with the Claude API** course has 14 assignments — verify you've completed all of them.

On Pluralsight: run skill assessments to identify remaining gaps. Adaptive assessments pinpoint weak areas in under 10 minutes.

### Days 3–4: Practice Questions
- Work through all **25 practice questions** on [Anthropic Certifications](https://www.anthropiccertifications.com/courses) WITHOUT looking at answers
- Score yourself first, then read every explanation — **including for questions you got right** (explanations contain additional nuance)
- For each wrong answer, write one sentence explaining why your answer was wrong and what the correct reasoning is

### Days 5–6: Scenario-Based Review
For each of the 6 exam scenarios, write out in your own words:
- What tools/agents are involved
- What the primary failure modes are
- What the correct architectural decisions are and **why**

This forces active recall rather than passive recognition.

### Day 7: Rest + Final Scan
Light review of the quick reference table below. For any term you can't define confidently, spend 15 minutes on it. Then rest.

---

## Exam Strategy

### The Distractor Patterns — Know These Cold

The exam consistently uses these wrong-answer types:

| Distractor Pattern | Why It's Wrong |
|-------------------|----------------|
| Prompt-based solutions to problems requiring programmatic enforcement | Prompts are probabilistic; financial/security constraints need deterministic enforcement |
| Over-engineered infrastructure when a simpler prompt fix suffices | Recognize when complexity is unnecessary |
| Fixing the downstream subagent when the coordinator decomposed incorrectly | Always trace errors to the source |
| Batch API for latency-sensitive workflows | Batch has no latency SLA — wrong tool for blocking checks |

### The Decision Rule
> **When unsure:** Is this problem deterministic or probabilistic?  
> For anything with **financial consequences or security implications**, programmatic enforcement (hooks, prerequisites) always beats prompt instructions.

### The Domain 1 Priority
Domain 1 is 27% of the exam. If you're short on time in Week 7, prioritize reviewing **agentic loops** and **multi-agent orchestration** over anything else.

---

## Quick Reference — Most Testable Concepts

| Concept | What You Must Know |
|---------|--------------------|
| `stop_reason: tool_use` | Continue the loop — execute tool and append result |
| `stop_reason: end_turn` | Done — terminate the loop |
| `tool_choice: "auto"` | Model MAY not call a tool |
| `tool_choice: "any"` | Model MUST call a tool |
| `tool_choice: {"type": "tool", "name": "..."}` | Specific tool guaranteed |
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
| "Lost in the middle" | Middle of context window gets dropped — keep critical facts at start/end |
| Progressive summarization | Loses numbers, dates, percentages — use persistent "case facts" block |

---

## Practice Resources

| Resource | Link | What It Covers |
|----------|------|----------------|
| ⭐ Anthropic Certifications Practice Site | [anthropiccertifications.com/courses](https://www.anthropiccertifications.com/courses) | 25 practice questions + 5 domain breakdowns for CCA-F — closest to official practice exam |
| Anthropic Skilljar (all courses) | [anthropic.skilljar.com](https://anthropic.skilljar.com) | All official Anthropic free courses |
| Coursera — Building with the Claude API | [coursera.org/learn/building-with-the-claude-api](https://www.coursera.org/learn/building-with-the-claude-api) | 14 assignments — complete all |

---

## To-Do List

### Days 1–2: Domain Review
- [ ] Run Pluralsight skill assessment — identify weak domains
- [ ] Review Week 2 notes (Domain 1 — Agentic Architecture, highest weight)
- [ ] Review Week 4 notes (Domain 3 — Claude Code)
- [ ] Review Week 5 notes (Domain 4 — Prompt Engineering)
- [ ] Review Week 3 notes (Domain 2 — Tool Design & MCP)
- [ ] Review Week 6 notes (Domain 5 — Context Management)
- [ ] Verify all 14 Coursera assignments completed
- [ ] Re-watch any shaky Coursera modules

### Days 3–4: Practice Questions
- [ ] Complete all 25 practice questions on [anthropiccertifications.com](https://www.anthropiccertifications.com/courses) (no peeking)
- [ ] Score yourself
- [ ] Read every explanation including for correct answers
- [ ] For each wrong answer: write one sentence on why it was wrong
- [ ] Review the 4 distractor patterns from the exam strategy section above

### Days 5–6: Scenario Review
- [ ] Write out Scenario 1 in your own words: agents, failure modes, correct decisions
- [ ] Write out Scenario 2 in your own words
- [ ] Write out Scenario 3 in your own words
- [ ] Write out Scenario 4 in your own words
- [ ] Write out Scenario 5 in your own words
- [ ] Write out Scenario 6 in your own words

### Day 7: Final Prep
- [ ] Review the Quick Reference table above — flag any term you can't confidently define
- [ ] Spend 15 minutes on each flagged term
- [ ] Confirm exam registration / logistics
- [ ] Rest — no new material
