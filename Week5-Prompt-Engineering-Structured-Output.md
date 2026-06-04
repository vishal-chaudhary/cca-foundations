---
layout: default
title: Week 5 — Prompt Engineering & Structured Output
description: Domain 4 — 20% of the exam
---

[← Back to Study Plan](./)

# Week 5 — Domain 4: Prompt Engineering & Structured Output (20%)

**Duration:** 6–8 hours  
**Goal:** Write precise prompts that produce consistent, validated structured output. Think of prompt engineering as writing precise API contracts for the model.

---

## Why This Week Matters

Domain 4 is 20% of the exam. Your technical background is an asset — "explicit criteria" is just specification writing, and "structured output via tool use" is typed API response design. The exam tests the nuances: when retries work vs. don't, which `tool_choice` setting guarantees a tool call, and what Batch API is and isn't suited for.

---

## Core Concepts

### Explicit Criteria (Task 4.1)
- Vague: *"check that comments are accurate"* → too ambiguous
- Precise: *"flag only when the claimed behavior contradicts the actual code"*
- `"Be conservative"` and `"high confidence only"` are useless without categorical definitions
- High false positive rates in one category undermine trust in ALL categories

### Few-Shot Prompting (Task 4.2)
- Most effective technique when instructions alone produce inconsistent results
- Show reasoning: **why was this action chosen** over the plausible alternative?
- Demonstrate handling of **ambiguous cases**, not just clean cases
- 2–4 targeted examples > 10+ generic ones

### Structured Output via Tool Use (Task 4.3)
| Approach | Reliability | Notes |
|----------|-------------|-------|
| `tool_use` + JSON schema | Highest | Eliminates syntax errors, but not semantic errors |
| `tool_choice: "auto"` | Medium | Model MAY return text instead of calling tool |
| `tool_choice: "any"` | High | Model MUST call a tool |
| `{"type": "tool", "name": "..."}` | Highest | Specific tool guaranteed |

- **Nullable fields** prevent hallucination when source data is absent
- Tool use eliminates syntax errors but does NOT prevent semantic errors (e.g., line items not summing to total)

### Validation & Retry Loops (Task 4.4)
- Retry with error feedback: append the validation error to the prompt on retry
- Retries work for: format mismatches, structural output errors
- Retries **DON'T** work for: information genuinely absent from source document
- `detected_pattern` field: track what code constructs triggered findings

### Batch Processing (Task 4.5)
| Feature | Value |
|---------|-------|
| Cost savings | 50% |
| Processing time | Up to 24 hours |
| Latency SLA | None |
| Multi-turn tool calling | Not supported |

**Use for:** overnight reports, weekly audits, nightly test generation  
**Do NOT use for:** blocking pre-merge checks (developers are waiting)

- `custom_id` fields for correlating request/response pairs

### Multi-Instance Review (Task 4.6)
- Self-review is less effective — model retains its own reasoning context
- Independent instance (no prior context) catches subtle issues better
- Multi-pass: per-file local analysis + separate cross-file integration pass

---

## Courses

### 🔴 Priority 1 — Anthropic Official

| Course | Platform | Scope This Week |
|--------|----------|-----------------|
| [Building with the Claude API](https://www.coursera.org/learn/building-with-the-claude-api) | Coursera (Anthropic) | **Module 3** — Claude features, tool use, structured data handling |

### 🟡 Priority 2 — Coursera Supplemental

| Course | Link | Why |
|--------|------|-----|
| Building RAG and MCP Servers with Claude (Edureka) | [coursera.org/learn/building-rag-and-mcp-servers-with-claude](https://www.coursera.org/learn/building-rag-and-mcp-servers-with-claude) | Structured output and validation patterns |

### 🔵 Pluralsight (Primary This Week)

| Course | Link | Focus |
|--------|------|-------|
| ⭐ Advanced Prompt Engineering | [pluralsight.com/courses/advanced-prompt-engineering](https://www.pluralsight.com/courses/advanced-prompt-engineering) | Few-shot, ReAct, Reflexion, prompt chaining |
| OpenAI Prompt Engineering for Improved Performance | [pluralsight.com/courses/improved-performance-openai-prompt-engineering](https://www.pluralsight.com/courses/improved-performance-openai-prompt-engineering) | Short (~41 min), good reinforcement |

### 🟢 Free Resources

| Resource | Link |
|----------|------|
| Anthropic Prompt Engineering Guide | [docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) |
| Few-Shot Prompting | [docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-examples](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-examples) |
| Tool Use with JSON Schemas | [docs.anthropic.com/en/docs/build-with-claude/tool-use/extracting-structured-json](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/extracting-structured-json) |
| Message Batches API | [docs.anthropic.com/en/docs/build-with-claude/batch-processing](https://docs.anthropic.com/en/docs/build-with-claude/batch-processing) |
| DeepLearning.AI — ChatGPT Prompt Engineering for Developers | [deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/) |

---

## Hands-On Exercise

Build a Structured Data Extraction Pipeline (mirrors an exam scenario):

1. Define an extraction tool with a JSON schema that includes:
   - Required fields
   - Optional fields
   - Nullable fields (for data that may be absent)
   - An enum field with an `"other"` fallback pattern

2. Force structured output using `tool_choice: {"type": "tool", "name": "extract_metadata"}`

3. Implement a **validation-retry loop**:
   - Validate the extracted output
   - On failure, append the specific validation error to the messages and retry
   - Cap retries at 3

4. Add 3 few-shot examples covering:
   - A clean, complete document
   - A document with missing optional fields
   - An ambiguous document

5. Submit a small batch of documents via the **Message Batches API** and retrieve results

---

## Exam Watch-Outs

| Wrong Answer Pattern | Correct Approach |
|----------------------|-----------------|
| `tool_choice: "auto"` when tool call is required | Use `tool_choice: "any"` or force specific tool |
| Retry when info is absent from source doc | Retries won't help — info genuinely isn't there |
| Use Batch API for pre-merge code review | Batch has no latency SLA — use synchronous for blocking checks |
| Omit nullable fields | Missing fields → hallucination risk |
| Use prose examples without showing reasoning | Show WHY the choice was made, not just the outcome |

---

## To-Do List

- [ ] Read [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) end-to-end
- [ ] Read [Few-Shot Prompting](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-examples)
- [ ] Read [Tool Use with JSON Schemas](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/extracting-structured-json)
- [ ] Read [Message Batches API](https://docs.anthropic.com/en/docs/build-with-claude/batch-processing)
- [ ] Complete Coursera: Building with the Claude API — **Module 3**
- [ ] Complete Pluralsight: ⭐ Advanced Prompt Engineering
- [ ] Complete Pluralsight: OpenAI Prompt Engineering for Improved Performance (~41 min)
- [ ] Watch DeepLearning.AI — ChatGPT Prompt Engineering for Developers (free)
- [ ] Build the Structured Data Extraction exercise:
  - [ ] Define JSON schema with required, optional, nullable, and enum fields
  - [ ] Force tool call with specific `tool_choice`
  - [ ] Implement validation-retry loop (max 3 retries)
  - [ ] Write 3 few-shot examples (clean, missing fields, ambiguous)
  - [ ] Submit batch via Message Batches API
- [ ] Know the 3 `tool_choice` modes and when to use each
- [ ] Know the Batch API limitations: no latency SLA, no multi-turn tool calling, 50% cost saving
- [ ] Write in your own words: when do retries help, and when don't they?
