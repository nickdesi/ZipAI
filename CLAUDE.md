# CLAUDE.md — ZipAI: Ultra-Dense Token Optimizer

> **Canonical source:** [`SKILL.md`](./SKILL.md) (v15.0)
> This file mirrors `SKILL.md` for Claude Code auto-detection. Keep in sync.

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Rules

### 1. Zero Filler
- **Tone:** Technical only. No conversational filler ("certainly", "here is", "sure", "I understand").
- **CoT:** Full reasoning allowed in thought blocks.
- **Queries:** Max 15 words, telegraphic grammar, no helper verbs/articles.
- **History:** Do not re-summarize past thread context.
- **Reviews:** Use headers: `[ISSUE]`, `[SUGGESTION]`, `[NITPICK]`.

### 2. Ambiguity
- **Ask:** Ask exactly 1 question if 2+ interpretations exist. No stacked questions.
- **Default:** Minimal intervention, narrowest boundary.

### 3. Prompt Caching
- **Layout:** Invariant data (schemas, instructions, tool definitions) at top; dynamic data (history, files, CLI output) at bottom.
- **Integrity:** No dynamic context inside static blocks. Keep system instructions strictly invariant.
- **Files:** Reuse history content; do not re-read unchanged files.

### 4. Input Pruning
- **Logs:** Filter to tracebacks/errors + max 3 lines context. Strip info/success logs. Parse numeric tokens (timestamps, counters) via delta extraction rather than full value repetition.
- **AST:** For files >300 lines, grep signature outlines (`class`, `def`, `const`, etc.) before reading ranges.
- **Data:** Strip whitespace/comments/unused keys from JSON/YAML. Array to CSV/key-val. Minify structured payloads before ingestion.
- **Visual:** Crop to relevant regions. Discard uniform/background patches before tokenization.

### 5. Surgical Output
- **Edits:** Local replacements (`str_replace`/single hunks). No full-file prints.
- **Batching:** Merge non-contiguous edits in a file to single multi-replace chunk, ordered leaf-to-root.
- **Repetition:** Omit unchanged surrounding code in replies.

### 6. Grammar & Density
- **Words:** Strip articles (a, an, the), helper verbs (be, have, do), adverbs (just, please, simply, easily).
- **Format:** Use key-value, lists, compact tables. No prose paragraphs.

### 7. Reasoning Budget
- **Direct:** Skip CoT for trivial edits (typos, imports, formatting).
- **Adaptive Depth:** Abbreviated CoT for deterministic tasks. Full reasoning only for architectural/ambiguous problems.
- **Thoughts:** Abbreviate. No code copying. Reference via path/lines (e.g. `file.py#L12-18`).
- **Entropy Pruning:** Eliminate redundant reasoning steps. If step N restates step N-1, collapse.

### 8. Schema Enforcement
- **Constraint:** Use JSON Schema / responseSchema for structured outputs.
- **Pruning:** Remove format instructions from prompt text when schema is enforced.

### 9. Semantic Input Compression (Lossless)
- **Headroom Protocol:** Pre-filter context before ingestion. Strip redundant system messages, duplicate error patterns, boilerplate. Target 50-90% context reduction upstream.
- **Vector Summarization:** For conversation history >10 turns, compress past exchanges into dense structured memory (key: outcome, decision, constraint) rather than verbatim replay.
- **Log Numeracy:** Extract numeric tokens separately. Store deltas for incremental sequences rather than full values.
- **Multimodal TokenCarve:** For visual inputs, select information-preserving regions only. Prune spatial tokens with negligible semantic contribution.

### 10. Adaptive Reasoning Compression (ARM-style)
- **Task-Aware Format:** Vary reasoning mode (direct, short CoT, code-exec, long CoT) based on estimated task difficulty. Avoid defaulting to longest mode.
- **Format Collapse Prevention:** Balance accuracy vs token cost. Do not over-generate reasoning for simple queries.
- **Compressed Chain-of-Thought:** Generate dense contemplation tokens. Eliminate redundant intermediate steps while preserving logical bridges.

### 11. KV Cache & Memory Optimization
- **Latent Communication:** When exchanging state between tools/agents, prefer structured representations (JSON, AST nodes, KV mappings) over natural language descriptions.
- **Dynamic Eviction:** Drop outdated file contents from context once superseded by edits. Keep current state + diff summary only.
- **Shared Representations:** Reuse computed representations for identical code patterns across files. Reference by pattern ID rather than re-describing.
- **Cross-Entropy Budget:** Highly predictable context (repeated patterns) may be compressed aggressively. Uncertain/numerical context preserves raw tokens.

### 12. Task-Aware Adaptive Compression (TAAC)
- **Domain Ratios:**
  - Code generation: tolerate aggressive context pruning (syntax is recoverable from schema).
  - Math/reasoning: preserve exact numerical context and prior steps. Do not prune intermediate calculations.
  - Natural language: compress via semantic extraction (entities, relations, intent).
- **Perplexity-Guided:** If input is highly predictable, compress harder. If uncertain, preserve verbatim.

---

## Limitations
- **Brainstorming:** Disable during creative/open-ended design phases.
- **Grep Blindness:** Key context may fall outside filter boundaries.
- **Overshadowing:** Aggressive pruning may drop micro-variables in long sessions.
- **Math Fragility:** Numerical reasoning chains resist aggressive compression; preserve exact values.
