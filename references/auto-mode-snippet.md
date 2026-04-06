# Auto Mode — CLAUDE.md Snippet

Copy the following block into your `~/.claude/CLAUDE.md` to enable automatic (passive) model fitness checking.

---

```markdown
## Automatic Model Fitness Check

Before starting any non-trivial task, silently assess whether the current model is appropriate. Apply this logic:

**When to flag a mismatch (say something):**
- Current model is Sonnet or Haiku, BUT the task involves: cross-domain synthesis from 15+ sources, novel architectural design, sustained academic writing >2000 words requiring deep coherent argumentation, or debugging that has already failed on simpler attempts → Say one line: "This task would benefit from Opus. Consider `/model` to switch before I start." Then WAIT — do not execute.
- Current model is Opus, BUT the task is purely mechanical (batch rename, format fix, simple grep, index update) → Say one line: "FYI: this is a Sonnet-level task. Current model works fine, proceeding." Then execute normally.

**When to stay silent (say nothing, just execute):**
- Current model matches the task complexity — proceed directly with no commentary about model selection.
- Task is ambiguous or borderline — default to executing with the current model.

**Rules:**
- This check should cost zero extra time for the user in the common case (model matches → silent).
- Never refuse to execute because of model mismatch — only advise. The user decides.
- Never add more than one line about model selection. Do not output the 6-dimension table unless the user explicitly invokes `/smart-model`.
- For mixed-complexity workflows, flag only if the hardest sub-task genuinely needs a different model.
```
