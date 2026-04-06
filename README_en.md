# Smart Model — Intelligent Model Selection for Claude Code

**[中文版](README.md)**

> Stop guessing which model to use. Let Claude decide — or decide for you, silently.

Smart Model analyzes task complexity across 6 dimensions and recommends the optimal Claude model (Opus / Sonnet / Haiku) before execution. It saves costs on simple tasks and prevents quality loss on complex ones.

---

## The Problem

Claude Code offers three model tiers with very different price/performance tradeoffs:

| Model | Strengths | Relative Cost |
|-------|-----------|---------------|
| **Opus 4.6** | Deep reasoning, cross-domain synthesis, long-form writing | 5x |
| **Sonnet 4.6** | Day-to-day coding, moderate synthesis, batch operations | 1x |
| **Haiku 4.5** | Quick lookups, syntax fixes, confirmations | 0.2x |

~80% of tasks are Sonnet-level, but users default to Opus "just in case" — wasting ~$63/month on 100 tasks. Conversely, using Sonnet for genuinely complex work produces noticeably weaker results.

## How It Works

### Two Modes

| Mode | Trigger | Behavior | Overhead |
|------|---------|----------|----------|
| **Automatic** | Every conversation (via CLAUDE.md) | Silent when model matches; one-line nudge when it doesn't | Zero in common case |
| **Manual** | `/smart-model <task>` | Full 6-dimension analysis table | On demand |

### The 6 Dimensions

| Dimension | What it measures |
|-----------|-----------------|
| **Reasoning depth** | Follow instructions vs. novel analysis |
| **Source breadth** | How many files/inputs to integrate |
| **Output creativity** | Mechanical operation vs. original composition |
| **Error cost** | Easy redo vs. cascading consequences |
| **Context demand** | Local scope vs. holding many files simultaneously |
| **Ambiguity** | Clear spec vs. defining the problem itself |

**Decision rule**: 2+ dimensions High → Opus. Most Low-Medium → Sonnet. All Low → Haiku.

## Installation

### 1. Copy the skill folder

```bash
cp -r smart-model ~/.claude/skills/
```

### 2. Enable automatic mode (recommended)

Append the content of [`references/auto-mode-snippet.md`](references/auto-mode-snippet.md) to your `~/.claude/CLAUDE.md`.

### 3. Done

- Automatic mode works immediately in all new conversations
- Manual mode: type `/smart-model <task description>` anytime

## Usage Examples

### Automatic mode (typical experience)

```
You (Sonnet): "Rename these 83 files and update wikilinks"
Claude: [silently proceeds — Sonnet is appropriate]

You (Sonnet): "Synthesize 40 papers into a cross-domain impact analysis"
Claude: "This task would benefit from Opus. Consider /model to switch before I start."

You (Opus): "Check how many files are in this folder"
Claude: "FYI: Sonnet-level task, proceeding with current model."
```

### Manual mode (`/smart-model`)

```
/smart-model Design a new database schema for the research wiki
```

Output:
```
Task Analysis
----------------------------------------------
Reasoning depth  : High — novel schema design with long-term consequences
Source breadth   : Med  — need to understand existing data patterns
Output creativity: High — original architecture, not template
Error cost       : High — schema mistakes cascade to all downstream code
Context demand   : Med  — several existing files to review
Ambiguity        : High — requirements underspecified

Recommendation   : Opus 4.6
Reason           : Architectural decision with lasting consequences, 3 High dimensions
Current model    : Sonnet 4.6
Action           : Switch recommended
```

## File Structure

```
smart-model/
├── SKILL.md                    ← Main skill file (Claude reads this)
├── README.md                   ← This file (English)
├── README_zh.md                ← Chinese documentation
├── LICENSE                     ← MIT License
├── .gitignore
├── templates/
│   └── analysis-output.md      ← Output format template
└── references/
    ├── model-capabilities.md   ← Detailed model comparison + cost analysis
    ├── decision-examples.md    ← 12 real-world decision cases
    └── auto-mode-snippet.md    ← CLAUDE.md snippet for automatic mode
```

## Quick Reference

### When does it recommend Opus?

- Cross-domain synthesis from 15+ sources requiring original insight
- Architectural decisions with long-term consequences
- Sustained academic writing >2000 words
- Debugging that has already failed on simpler attempts
- Tasks where the user signals high stakes

### When does it stay with Sonnet?

- Feature implementation, code generation, refactoring
- Batch operations regardless of file count (scale ≠ complexity)
- Moderate synthesis (≤12 bounded sources)
- Agent orchestration and dispatch
- Most day-to-day software engineering

### When does it suggest Haiku?

- Single-command lookups (`ls`, `grep`, `wc`)
- Syntax fixes, formatting corrections
- "Did that work?" validation checks

## FAQ

**Q: Will automatic mode slow down every interaction?**
A: No. When the model matches (the common case), it's completely silent — zero extra output.

**Q: Can it block me from using a model?**
A: Never. It only advises. You always have the final decision.

**Q: Does "important" mean "use Opus"?**
A: No. Importance ≠ complexity. A critical but simple config change is still Sonnet-level. Opus is for tasks requiring deep reasoning, not high stakes.

**Q: What about mixed-complexity workflows?**
A: It suggests splitting: "Phase 1 (data extraction) → Sonnet. Phase 2 (synthesis) → Opus."

## License

MIT — see [LICENSE](LICENSE).
