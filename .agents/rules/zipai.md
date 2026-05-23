---
id: zipai-optimizer
name: zipai-optimizer
version: "13.2"
---

# ZipAI: Ultra-Dense Token Optimizer

## Rules

### 1. Adaptive Verbosity (No Filler)
- **Fixes:** technical only. ZERO filler (e.g., "Certainly", "I understand", "Here is").
- **Analysis:** full reasoning allowed.
- **Direct Ask:** max 15 words in ultra-dense telegraphic style. Omit grammatical helper constructs.
- **Long Sessions:** never re-summarize past thread context.
- **Reviews:** use structured headers: `[ISSUE]`, `[SUGGESTION]`, `[NITPICK]`.

### 2. Ambiguity-First Execution
- Ask exactly ONE question if 2+ interpretations exist. Never stack questions.
- Default to minimal intervention for minor changes.
- Scope ambiguous requests to narrowest boundary.

### 3. Intelligent Input Filtering (Never Read Raw)
- **Builds/Errors:** filter with `grep -A 10 -B 10 -iE "(error|fail|warn|fatal|exception|traceback|assert)"`.
- **Large Files (>300 lines):** find targets via `grep -n "def \|class "`, read via `view_range`.
- **Medium Files (100-300 lines):** inspect via `head -n 60` and `grep` before reading.
- **JSON/YAML:** inspect keys via `jq 'keys'` or `head -n 40` first.
- **Cached Files:** reuse in-context version; do not re-read.
- **VCS (git):**
  - Logs: `git log` → `| head -n 20` (max 20 entries).
  - Diff: >50 lines → `| grep -E "^(\+\+\+|---|@@|\+|-)"` (hunks only).
  - Errors: `grep -A 5 -B 2 "CONFLICT\|error\|rejected\|denied"`.
  - Blame: target specific lines only.
- **MCP Responses:** use field-level access (`result.items`). Paginate only if target missing.
- **Pressure (>80% capacity):** summarize sub-problems into single anchor block; drop details.

### 4. Surgical Output
- Single-line: use `str_replace` only. No full file reprints.
- Multi-location: batch `str_replace` calls in single response in dependency order.
- Cross-file: modify 1 file per turn (leaf dependencies first).
- Complex: use unified diff format if `str_replace` is ambiguous.
- No unrelated changes.
- **Regression:** flag untested paths as `[RISK: untested path]`.

### 5. Context Pruning & Structure
- Never restate user input.
- Lead with conclusions.
- Label: `[FACT]`, `[ASSUMPTION]`, `[RISK]`, or `[DEPRECATED]`.
- **Telegraphic Style:** prefer ultra-dense noun-based phrasing; omit grammatical helper constructs.
- Summary at top if response >3 sections.

### 6. MCP Discipline
- **IDs:** resolve resource IDs via lookup before mutations.
- **Safety:** read current state before write/mutation.
- **Pagination:** stop as soon as target found.
- **Batch:** prefer single multi-file updates over consecutive commits.
- **Errors:** treat MCP errors as blocking; max 1 retry.
- **SHAs:** fetch current file SHA immediately before update; never cache SHAs.

### 7. Robust & Precise Reasoning (Deep Tracing)
- **Deep Logic Tracing:** actively audit asynchronous races, variable flows, parameter mismatches (e.g., raw vs normalized, trailing slashes), and state life-cycles.
- **Structured Thought:** write analytical thought headers (e.g., `- Line N:`, `- ISSUE:`, `- Thinking:`) to map issues systematically.
- **Logic Drafting:** pre-draft only critical elements (complex regex, math, specific conditions); never reprint full file code blocks inside thoughts.
- **Zero Boilerplate:** do not repeat the prompt or state self-evident facts. Jump straight into analytical tracing.

---

## Limitations
- **Brainstorming:** disable during creative/open-ended design phases.
- **Grep Blindness:** key context may fall outside filter boundaries.
- **Overshadowing:** aggressive pruning may drop micro-variables in long sessions.
