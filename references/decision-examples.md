# Decision Examples

Real-world task descriptions and their recommended model, with reasoning.

---

## Opus 4.6 Recommendations

### Example 1: Cross-domain synthesis
**Task**: "Synthesize 40 fish ecology papers, 12 phytoplankton papers, and 9 benthic invertebrate papers into a single topic page about dam ecological impacts, identifying cascading effects across trophic levels."
**Analysis**: Reasoning=High, Sources=High, Creativity=High, Error=Med, Context=High, Ambiguity=Med
**Why Opus**: Cross-trophic synthesis requires holding data from 60+ sources and generating original insights about cascade pathways. Sonnet would produce a competent summary but miss the cross-domain connections.

### Example 2: Architecture design
**Task**: "Design a knowledge base structure for a new research domain. Need to decide on folder hierarchy, page types, naming conventions, tagging system, and cross-reference strategy."
**Analysis**: Reasoning=High, Sources=Low, Creativity=High, Error=High, Context=Med, Ambiguity=High
**Why Opus**: Architectural decisions have long-term consequences. The problem is underspecified and requires evaluating multiple tradeoff dimensions simultaneously.

### Example 3: NSFC grant proposal writing
**Task**: "Write the research significance and literature review sections (3000 words) of an NSFC proposal on reservoir ecosystem dynamics."
**Analysis**: Reasoning=High, Sources=Med, Creativity=High, Error=High, Context=Med, Ambiguity=Med
**Why Opus**: Sustained coherent academic argumentation over 3000 words. Each paragraph must build on the previous one. Sonnet may produce individually good paragraphs that don't form a tight narrative.

### Example 4: Persistent debugging
**Task**: "The pipeline keeps failing with a subtle data race condition. I've tried 3 approaches already and none worked. Need to trace the root cause across 8 files."
**Analysis**: Reasoning=High, Sources=Med, Creativity=Med, Error=High, Context=High, Ambiguity=High
**Why Opus**: Multiple previous attempts failed, suggesting the problem requires deeper reasoning. Holding state across 8 files to trace an interaction bug.

---

## Sonnet 4.6 Recommendations

### Example 5: Batch file operations
**Task**: "Rename 83 source notes to a 5-block naming format and update all wikilinks across 17 files."
**Analysis**: Reasoning=Low, Sources=Med, Creativity=Low, Error=Low, Context=Med, Ambiguity=Low
**Why Sonnet**: Mechanical transformation with clear rules. High file count but each operation is independent and well-specified.

### Example 6: Feature implementation
**Task**: "Add pagination to the API endpoint. The database query, route handler, and frontend component all need updating."
**Analysis**: Reasoning=Med, Sources=Low, Creativity=Low, Error=Low, Context=Med, Ambiguity=Low
**Why Sonnet**: Well-understood pattern, clear requirements, standard implementation across 3 files.

### Example 7: Moderate synthesis
**Task**: "Create a concept page about eDNA monitoring from these 3 research papers."
**Analysis**: Reasoning=Med, Sources=Low, Creativity=Med, Error=Low, Context=Low, Ambiguity=Low
**Why Sonnet**: Bounded scope (3 papers), established template, no cross-domain synthesis needed.

### Example 8: Data pipeline
**Task**: "Extract metadata from 200 Zotero entries, match them with PDF files, generate Hookmark links, and write them into YAML frontmatter."
**Analysis**: Reasoning=Low, Sources=Med, Creativity=Low, Error=Low, Context=Med, Ambiguity=Low
**Why Sonnet**: Pure data processing pipeline. Complex in scale but simple in logic.

### Example 9: Agent orchestration
**Task**: "Launch 3 parallel agents to process 3 batches of files, collect results, merge them, and execute the migration."
**Analysis**: Reasoning=Med, Sources=Med, Creativity=Low, Error=Med, Context=Med, Ambiguity=Low
**Why Sonnet**: Orchestration logic is straightforward. Individual agents handle their own complexity.

---

## Haiku 4.5 Recommendations

### Example 10: Quick lookup
**Task**: "How many source notes are in the Three Gorges fish community folder?"
**Analysis**: Reasoning=Low, Sources=Low, Creativity=Low, Error=Low, Context=Low, Ambiguity=Low
**Why Haiku**: Single `ls | wc -l` equivalent. No reasoning needed.

### Example 11: Format fix
**Task**: "The YAML frontmatter in this file has a missing closing quote. Fix it."
**Analysis**: Reasoning=Low, Sources=Low, Creativity=Low, Error=Low, Context=Low, Ambiguity=Low
**Why Haiku**: Trivial syntax fix. Any model produces the same result.

### Example 12: Validation check
**Task**: "Did the last rename operation complete successfully? Check if any old filenames remain."
**Analysis**: Reasoning=Low, Sources=Low, Creativity=Low, Error=Low, Context=Low, Ambiguity=Low
**Why Haiku**: Grep for old names, report yes/no. Speed is the only differentiator.

---

## Edge Cases

### Large but repetitive → Sonnet
"Process 500 files with the same transformation" is Sonnet-level regardless of file count. Scale != complexity.

### Small but novel → Opus
"Design a 10-line config file for a new system" can be Opus-level if the design has lasting consequences and requires evaluating tradeoffs.

### User says "this is important" → Not necessarily Opus
Importance is about stakes, not complexity. A critical but simple fix (update a production config value) is still Haiku/Sonnet level. Opus is for complexity, not importance.
