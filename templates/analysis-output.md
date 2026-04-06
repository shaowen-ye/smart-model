# Analysis Output Template

When `/smart-model` is invoked explicitly, use this format:

```
Task Analysis
----------------------------------------------
Reasoning depth  : {Low/Med/High} — {brief reason}
Source breadth   : {Low/Med/High} — {brief reason}
Output creativity: {Low/Med/High} — {brief reason}
Error cost       : {Low/Med/High} — {brief reason}
Context demand   : {Low/Med/High} — {brief reason}
Ambiguity        : {Low/Med/High} — {brief reason}

Recommendation   : {Opus 4.6 / Sonnet 4.6 / Haiku 4.5}
Reason           : {one sentence}
Current model    : {detect from environment}
Action           : {Proceed / Switch recommended / Proceed (overqualified)}
```

## Rules for the output

- Each dimension gets ONE line with a short justification (5-15 words)
- Do NOT add extra commentary or caveats beyond the format
- The "Action" field determines what happens next:
  - `Proceed` → start the task immediately after the table
  - `Switch recommended` → print switch instruction, do NOT start
  - `Proceed (overqualified)` → note cost, start immediately

## For automatic mode (CLAUDE.md passive check)

Do NOT use this template. Output at most ONE line:

- Match → say nothing
- Need stronger model → "This task would benefit from Opus. Consider `/model` to switch before I start."
- Overqualified → "FYI: Sonnet-level task, proceeding with current model."
