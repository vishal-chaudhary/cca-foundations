---
layout: default
title: Week 1 — Foundations
description: Claude API, LLM Basics & Environment Setup
---

[← Back to Study Plan](./)

# Week 1 — Foundations: Claude API, LLM Basics & Environment Setup

**Duration:** 6–8 hours  
**Goal:** Get your hands on the API. Make your first calls. Understand the request/response lifecycle before anything else.

---

## What You're Learning This Week

How LLMs work under the hood is less important than knowing how to *use* them correctly. Your architecture instincts will help — think of the Claude API as a stateless service where you manage conversation state explicitly. The key insight this week: every API call is independent, and the `stop_reason` field tells you what to do next.

### Core Concepts

- How LLMs work at a high level — tokens, context windows, temperature
- Claude API request structure: `messages`, `system`, `max_tokens`, `stop_reason`
- The difference between `end_turn` and `tool_use` stop reasons (critical for all later weeks)
- Setting up your dev environment (Python recommended)
- Reading the full response object — don't just extract the text content

---

## Courses

### 🔴 Priority 1 — Anthropic Official (Do First)

| Course | Platform | Scope This Week |
|--------|----------|-----------------|
| [Building with the Claude API](https://www.coursera.org/learn/building-with-the-claude-api) | Coursera (Anthropic) | **Modules 1–2 only** — API fundamentals + prompt engineering basics |

> Save Modules 3–7 for later weeks. This course spans the entire 7-week plan.

### 🟡 Priority 2 — Free Supplemental

| Resource | Link | Notes |
|----------|------|-------|
| Anthropic API Quickstart | [docs.anthropic.com/en/api/getting-started](https://docs.anthropic.com/en/api/getting-started) | Start here before anything else |
| Messages API Reference | [docs.anthropic.com/en/api/messages](https://docs.anthropic.com/en/api/messages) | Read the full request/response schema |
| Models Overview | [docs.anthropic.com/en/docs/about-claude/models/overview](https://docs.anthropic.com/en/docs/about-claude/models/overview) | Understand what `claude-sonnet-4-6` etc. mean |
| Anthropic Skilljar — Building with the Claude API | [anthropic.skilljar.com/claude-with-the-anthropic-api](https://anthropic.skilljar.com/claude-with-the-anthropic-api) | Free, same content as Coursera — use as backup |
| Andrej Karpathy — Intro to LLMs (1hr) | [youtube.com/watch?v=zjkBMFhNj_g](https://www.youtube.com/watch?v=zjkBMFhNj_g) | Best conceptual foundation available |
| DeepLearning.AI — Building Systems with ChatGPT API | [deeplearning.ai/short-courses/building-systems-with-chatgpt/](https://www.deeplearning.ai/short-courses/building-systems-with-chatgpt/) | Free, concepts transfer directly to Claude |

### 🔵 Pluralsight (Supplemental, ~1hr)

| Course | Link |
|--------|------|
| Anthropic: Prompt Engineering for Developers | [pluralsight.com/courses/anthropic-prompt-engineering-developers](https://www.pluralsight.com/courses/anthropic-prompt-engineering-developers) |

Good quick primer on Claude specifically before diving into the API.

---

## Hands-On Exercise

Write a Python (or Node.js) script that does all of the following:

1. Makes a basic Claude API call with a `system` prompt
2. Logs the **full response object** including `stop_reason`
3. Tries different values of `max_tokens` and observes the truncation effect
4. Sends a multi-turn conversation (manual message array management)

```bash
# Install SDK
pip install anthropic        # Python
npm install @anthropic-ai/sdk  # Node
```

SDK reference: [github.com/anthropic/anthropic-sdk-python](https://github.com/anthropic/anthropic-sdk-python)

Get your API key: [console.anthropic.com](https://console.anthropic.com) (~$5 of credits covers all exercises)

---

## Key Concepts to Lock In

| Concept | What to Know |
|---------|-------------|
| `stop_reason: end_turn` | Model finished naturally — you're done |
| `stop_reason: tool_use` | Model wants to call a tool — continue the loop |
| `stop_reason: max_tokens` | Response was cut off — increase limit or handle truncation |
| `system` field | Sets the model's persona/instructions — separate from `messages` |
| `max_tokens` | Hard cap on output length — not a target |
| Context window | Total token budget for input + output combined |

---

## To-Do List

- [ ] Get API key from [console.anthropic.com](https://console.anthropic.com)
- [ ] Install the Anthropic Python SDK (`pip install anthropic`)
- [ ] Read the [API Quickstart](https://docs.anthropic.com/en/api/getting-started) end-to-end
- [ ] Read the [Messages API reference](https://docs.anthropic.com/en/api/messages) — understand the full request schema
- [ ] Read the [Models Overview](https://docs.anthropic.com/en/docs/about-claude/models/overview)
- [ ] Start Coursera: **Building with the Claude API** — complete Module 1 (API fundamentals)
- [ ] Complete Module 2 (prompt engineering basics)
- [ ] Watch Andrej Karpathy Intro to LLMs video (1hr)
- [ ] Complete Pluralsight: Anthropic Prompt Engineering for Developers (~1hr)
- [ ] Write the hands-on script: basic API call with system prompt + log full response
- [ ] Extend script: vary `max_tokens`, observe truncation
- [ ] Extend script: send a multi-turn conversation manually
- [ ] Verify you understand the difference between `end_turn` and `tool_use` stop reasons
