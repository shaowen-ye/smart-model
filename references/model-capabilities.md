# Model Capabilities Reference

Detailed comparison of Claude model tiers for task routing decisions.

---

## Opus 4.6

**Architecture**: Largest Claude model, 1M context window

**Strengths**:
- Deep multi-step reasoning chains where early errors cascade
- Holding and synthesizing information from 20+ files simultaneously
- Generating coherent long-form text (>2000 words) with consistent argumentation
- Resolving contradictions between sources
- Novel architectural/design decisions with long-term consequences
- Understanding complex interactions in debugging scenarios
- Nuanced judgment calls in ambiguous situations

**Weaknesses**:
- ~5x cost of Sonnet per token
- Slower first-token latency
- Overkill for well-specified, mechanical tasks
- No quality advantage for template-based work

**Best for**: Tasks where a wrong answer is expensive and a right answer requires genuine insight.

---

## Sonnet 4.6

**Architecture**: Balanced Claude model, same 1M context window

**Strengths**:
- Fast, cost-effective for most software engineering tasks
- Strong code generation and refactoring
- Reliable at following well-specified instructions
- Good at moderate synthesis (bounded scope, <15 sources)
- Excellent at batch operations and parallel agent orchestration
- More than sufficient for template-based content creation

**Weaknesses**:
- May produce shallower analysis on deeply cross-domain tasks
- Long-form coherent argumentation can be less tight than Opus
- May miss subtle contradictions when holding many files in context
- Less reliable at novel architectural decisions

**Best for**: The 70% of tasks that are well-specified, bounded in scope, and recoverable if imperfect.

---

## Haiku 4.5

**Architecture**: Smallest Claude model, fastest response

**Strengths**:
- Extremely fast response times
- Very low cost (~0.2x of Sonnet)
- Sufficient for lookups, confirmations, and simple transformations
- Good for rapid iteration loops where speed > first-try quality

**Weaknesses**:
- Limited reasoning depth
- May miss nuances in complex instructions
- Not suitable for multi-step tasks
- Lower quality on any task requiring judgment

**Best for**: Quick confirmations, file lookups, syntax fixes, and rapid prototyping iterations.

---

## Decision Boundaries

### Sonnet → Opus upgrade signals

These patterns during execution suggest the task actually needs Opus:
- Finding yourself reading 15+ files and struggling to hold them together
- Producing synthesis that feels like "summary concatenation" rather than insight
- Making the same type of error repeatedly despite corrections
- User feedback indicating the output lacks depth or coherence

### Sonnet → Haiku downgrade signals

These patterns suggest Haiku would suffice:
- The entire task is a single tool call (grep, file read, rename)
- No judgment required — purely mechanical transformation
- User is iterating rapidly and wants speed over polish

---

## Cost Comparison (approximate, as of 2025)

| Model | Input ($/1M tokens) | Output ($/1M tokens) | Relative |
|-------|---------------------|----------------------|----------|
| Opus 4.6 | $15 | $75 | 5x |
| Sonnet 4.6 | $3 | $15 | 1x |
| Haiku 4.5 | $0.80 | $4 | 0.25x |

A typical complex task (50k input + 5k output):
- Opus: ~$1.13
- Sonnet: ~$0.23
- Haiku: ~$0.06

Over 100 tasks/month, choosing Sonnet over Opus for 70 of them saves ~$63/month.
