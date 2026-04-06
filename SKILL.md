---
name: smart-model
description: "Intelligent model selection for Claude Code. Analyzes task complexity across 6 dimensions, recommends the optimal model (Opus/Sonnet/Haiku), and either proceeds or advises switching. Use when starting a new task, planning a multi-step workflow, or unsure which model fits. Trigger on: '/smart', 'which model', 'model select', 'smart model', '模型选择', '选模型', '用哪个模型', 'should I use opus', 'switch model?'"
user_invocable: true
---

# Smart Model Selection

Analyze task complexity and recommend the optimal Claude model before execution. Prevents wasting Opus tokens on mechanical tasks and avoids under-powering complex reasoning with lighter models.

## Task to Analyze

$ARGUMENTS

If no arguments provided, ask: "Describe the task you're about to do, and I'll recommend the best model."

## Analysis Framework

Evaluate the task across **6 dimensions**, each scored Low / Medium / High:

| Dimension | Low | Medium | High |
|-----------|-----|--------|------|
| **Reasoning depth** | Follow instructions, apply patterns | Moderate judgment, choose between approaches | Novel analysis, resolve contradictions, build arguments |
| **Source breadth** | 0-3 files/inputs | 4-15 files/inputs | 16+ files, cross-domain integration |
| **Output creativity** | Mechanical (rename, format, migrate) | Templated with adaptation (notes, standard code) | Original composition (papers, architecture, synthesis) |
| **Error cost** | Easily re-done, low stakes | Moderate rework if wrong | Hard to detect errors, cascading consequences |
| **Context demand** | Fits in short context, local scope | Needs moderate file reading | Requires holding many files simultaneously |
| **Ambiguity** | Clear spec, well-defined steps | Some judgment calls needed | Underspecified, requires framing the problem itself |

## Decision Rules

### Opus 4.6 — when depth matters

Recommend if **2+ dimensions are High**, OR any ONE of:
- Cross-domain synthesis requiring original insight (not just aggregation)
- Architectural decisions with long-term consequences
- Debugging complex emergent behavior after simpler attempts failed
- Academic/professional writing requiring sustained coherent argumentation (>2000 words)
- Planning that requires evaluating tradeoffs across multiple constraints
- Tasks where the user explicitly signals high stakes ("this is critical", "get this right")

### Sonnet 4.6 — the default workhorse

Recommend if **most dimensions are Low-Medium**, including:
- Feature implementation from clear requirements
- Code generation, refactoring, test writing
- Moderate synthesis or summarization (bounded scope)
- Batch operations, file processing, data transformation
- Template-based content creation
- Database operations, API integration
- Agent orchestration (dispatching sub-tasks to parallel agents)
- Most day-to-day software engineering tasks

### Haiku 4.5 — speed over depth

Recommend if **all dimensions are Low**, including:
- Quick lookups: file existence, line counts, grep results
- Syntax fixes, formatting corrections, typo repairs
- Simple Q&A about codebase ("where is X defined?")
- Rapid prototyping where iteration speed > first-try quality
- Validation checks ("did that command work?")

## Output Format

```
Task Analysis
----------------------------------------------
Reasoning depth : {Low/Med/High} — {brief reason}
Source breadth  : {Low/Med/High} — {brief reason}
Output creativity: {Low/Med/High} — {brief reason}
Error cost      : {Low/Med/High} — {brief reason}
Context demand  : {Low/Med/High} — {brief reason}
Ambiguity       : {Low/Med/High} — {brief reason}

Recommendation  : {Opus 4.6 / Sonnet 4.6 / Haiku 4.5}
Reason          : {one sentence}
Current model   : {detect from environment}
Action          : {Proceed / Switch first}
```

## Execution Logic

**Case 1: Recommended = Current model**
→ Output analysis, then say "Proceeding." and **start the task immediately**.

**Case 2: Recommended is MORE capable than current** (e.g., current Sonnet → need Opus)
→ Output analysis, then say:
"Switch recommended: run `/model` and select {recommended model}, then describe the task again."
→ Do NOT execute the task. Wait for the user.

**Case 3: Recommended is LESS capable than current** (e.g., current Opus → Sonnet sufficient)
→ Output analysis, then say:
"Current model can handle this (though {recommended} would be more cost-efficient). Proceeding with current model."
→ **Start the task immediately** unless user objects.

## Special Cases

### Mixed-complexity workflows
If the task has distinct phases at different complexity levels, recommend splitting:
"Phase 1 (data extraction) → Sonnet. Phase 2 (cross-domain synthesis) → Opus. Suggest completing Phase 1 first, then switching."

### Parallel agent dispatch
If the task can be parallelized across agents:
"Main thread: Sonnet (orchestration). Sub-agents: can use model override if individual sub-tasks need Opus."

### Uncertain complexity
If you genuinely can't assess complexity from the description alone:
"Insufficient detail to assess. Starting with Sonnet — will flag if the task turns out to need Opus."

## Rules

- Default to Sonnet when uncertain — it's the safe middle ground
- Never recommend Opus just because a task is "important" — importance != complexity
- A large but repetitive task (e.g., rename 100 files) is still Sonnet-level
- A small but novel task (e.g., design a new taxonomy) may need Opus
- Do not over-explain — keep the analysis concise, the user wants a quick decision
