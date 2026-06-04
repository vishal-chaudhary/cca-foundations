---
layout: default
title: Week 6 — Context Management & Reliability
description: Domain 5 — 15% of the exam
---

[← Back to Study Plan](index.md)

# Week 6 — Domain 5: Context Management & Reliability (15%)

**Duration:** 6–8 hours  
**Goal:** Understand how context degrades over long sessions, how errors must propagate across multi-agent systems, and how to design reliable human-review workflows.

---

## Why This Week Matters

Domain 5 is the lightest domain by weight (15%), but these concepts appear throughout every scenario in the exam. Context management mistakes are the most common source of subtle bugs in production agentic systems — and the exam knows it.

---

## Core Concepts

### Context Preservation (Task 5.1)
Progressive summarization **loses**:
- Numerical values, dates, percentages
- Customer expectations set earlier in conversation
- Specific commitments made

**"Lost in the middle" effect**: models process start and end reliably; the middle gets dropped.

Best practices:
- Trim tool results to relevant fields only (don't append full API responses)
- Maintain a persistent "case facts" block: extract key transactional data outside summarized history
- Use `/compact` during extended sessions to reduce context usage

### Escalation Patterns (Task 5.2)
**Always escalate immediately when:**
- Customer explicitly requests a human
- Policy is silent or ambiguous on the specific request

**Never escalate based on:**
- Sentiment alone (unreliable proxy for complexity)

**Never guess:**
- Multiple customer matches → ask for more identifiers, never pick one

### Error Propagation (Task 5.3)
Structured error context must include:
- Failure type
- Attempted query
- Partial results (what did work)
- Alternatives tried or available

Key rules:
- Access failures ≠ valid empty results — distinguish them clearly
- Subagents handle transient failures locally; propagate only what they can't resolve
- **Never suppress errors silently** (returning empty as success = anti-pattern)

### Large Codebase Context (Task 5.4)
Signs of **context degradation**:
- Model starts referencing "typical patterns" instead of actual classes in the codebase
- Responses become generic rather than specific

Mitigation strategies:
- Scratchpad files: persist key findings across context boundaries
- Subagent delegation: isolate verbose exploration output from main coordination
- Structured state persistence for crash recovery (manifest files)
- `/compact` to reduce context usage during extended sessions

### Human Review Workflows (Task 5.5)
- 97% aggregate accuracy can mask 60% accuracy on specific document types
- **Stratified random sampling** for ongoing error rate measurement
- Field-level confidence scores calibrated against labeled validation sets
- Route to human review when: low confidence **OR** ambiguous/contradictory source documents

### Information Provenance (Task 5.6)
- Source attribution is **lost during summarization** unless explicitly preserved
- Use structured claim-source mappings: `claim + source URL + excerpt + publication date`
- Conflicting statistics from credible sources → annotate BOTH with attribution, don't pick one
- Temporal data: always include publication/collection dates to distinguish contradiction from temporal difference

---

## Courses

### 🔴 Priority 1 — Anthropic Official

| Course | Platform | Scope This Week |
|--------|----------|-----------------|
| [Building with the Claude API](https://www.coursera.org/learn/building-with-the-claude-api) | Coursera (Anthropic) | **Module 6** — Agentic workflows, context passing, session management |

Complete any remaining modules from this course this week.

### 🟢 Free Resources

| Resource | Link |
|----------|------|
| Long Context Best Practices | [docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips) |
| DeepLearning.AI — Building and Evaluating Advanced RAG | [deeplearning.ai/short-courses/building-evaluating-advanced-rag/](https://www.deeplearning.ai/short-courses/building-evaluating-advanced-rag/) |

### 🔵 Pluralsight (Primary This Week)

| Course | Link | Focus |
|--------|------|-------|
| Evaluating and Optimizing LLM Agents | [pluralsight.com/courses/evaluating-optimizing-llm-agents](https://www.pluralsight.com/courses/evaluating-optimizing-llm-agents) | Reliability, confidence calibration, error handling |
| Agentic AI Safety and Alignment | [pluralsight.com/courses/agentic-ai-safety-alignment](https://www.pluralsight.com/courses/agentic-ai-safety-alignment) | Escalation, human-in-the-loop, guardrails |
| Frameworks for Developing LLM Agents | [pluralsight.com/courses/frameworks-developing-llm-agents](https://www.pluralsight.com/courses/frameworks-developing-llm-agents) | Long-term conversation, state management |

---

## Hands-On Exercise

Build a Multi-Agent Research Pipeline:

1. **Coordinator with 2+ subagents** — explicit context injection into each subagent prompt
2. **Parallel subagent execution** — emit multiple `Task` calls in a single coordinator response
3. **Structured output with claim-source mappings**:
   ```json
   {
     "claim": "...",
     "source_url": "...",
     "excerpt": "...",
     "publication_date": "..."
   }
   ```
4. **Simulate a timeout** — verify structured error propagation (not silent suppression)
5. **Feed conflicting data** from two sources — verify both values are preserved with attribution, not arbitrarily resolved

---

## Exam Watch-Outs

| Wrong Answer Pattern | Correct Approach |
|----------------------|-----------------|
| Summarize context without preserving numbers/dates | Use persistent "case facts" block outside summary |
| Escalate based on negative sentiment | Escalate only on explicit human request or policy silence |
| Return empty result when API access fails | Return structured error distinguishing access failure from empty result |
| Pick one value when sources conflict | Annotate both with attribution |
| 97% aggregate accuracy = good enough | Check per-category accuracy — 97% aggregate can mask 60% per-type |
| Suppress subagent errors | Propagate what can't be locally resolved |

---

## To-Do List

- [ ] Read [Long Context Best Practices](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips)
- [ ] Complete Coursera: Building with the Claude API — **Module 6** (and any remaining modules)
- [ ] Complete Pluralsight: Evaluating and Optimizing LLM Agents (focus on reliability sections)
- [ ] Complete Pluralsight: Agentic AI Safety and Alignment (focus on escalation + human-in-the-loop)
- [ ] Complete Pluralsight: Frameworks for Developing LLM Agents (focus on state management)
- [ ] Watch DeepLearning.AI — Building and Evaluating Advanced RAG (free)
- [ ] Build the Multi-Agent Research Pipeline:
  - [ ] Coordinator + 2 subagents with explicit context injection
  - [ ] Parallel subagent spawning in one coordinator response
  - [ ] Claim-source mapping output structure
  - [ ] Simulate timeout → verify structured error propagation
  - [ ] Feed conflicting sources → verify both values preserved
- [ ] Know the "lost in the middle" effect and its implications
- [ ] Know exactly when to escalate to a human (and when NOT to)
- [ ] Know why aggregate accuracy metrics can be misleading
- [ ] Write in your own words: how do you preserve source attribution through summarization?
