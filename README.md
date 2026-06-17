# ZipAI: Ultra-Dense Token Optimizer

[![Version](https://img.shields.io/badge/version-15.0-blue.svg)](./SKILL.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)

ZipAI is a set of optimization rules and prompt frameworks designed to minimize LLM input (prefill) and output (generation) token consumption. It maximizes **Prompt Caching** efficiency, enforces **Semantic Input Pruning** (inspired by LLMLingua-2), and guides LLM agents toward ultra-dense, telegraphic interactions.

---

## Key Features

| # | Feature | Mechanism |
|---|---|---|
| 1 | **Zero Filler** | Strips conversational padding, enforces telegraphic grammar |
| 2 | **Prompt Caching** | Static-first ordering preserves prefix cache hits (up to 90%) |
| 3 | **Input Pruning** | Log compression, AST skeletal inspection, JSON/YAML minification |
| 4 | **Surgical Output** | Localized diffs only, no full-file reprints |
| 5 | **Reasoning Budget** | Adaptive CoT depth based on task complexity |
| 6 | **Schema Enforcement** | JSON Schema replaces verbose format instructions |
| 7 | **Semantic Compression** | Lossless context reduction targeting 50-90% savings |
| 8 | **ARM-style Reasoning** | Task-aware reasoning mode selection |
| 9 | **KV Cache Optimization** | Structured inter-tool communication, dynamic eviction |
| 10 | **TAAC** | Domain-specific compression ratios (code vs math vs NL) |

---

## Token Savings Benchmark

| Category | Optimization | Before | After | Reduction |
|---|---|---|---|---|
| **Log Files** | Traceback extraction + context | ~4,500 tokens | ~225 tokens | **-95.0%** |
| **MCP Payloads** | Minified JSON keys-only | ~1,800 tokens | ~180 tokens | **-90.0%** |
| **Code Inspection** | AST Grep + targeted line view | ~6,200 tokens | ~480 tokens | **-92.2%** |
| **Session Context** | Static-First caching | 30,000 tokens | ~3,000 equivalent | **-90.0%** (cache) |
| **Agent Generation** | Telegraphic + surgical diffs | ~450 tokens | ~95 tokens | **-78.8%** |

---

## Installation

### Antigravity IDE (Gemini Agent Skill)

```bash
mkdir -p ~/.gemini/config/skills/zipai
cp SKILL.md ~/.gemini/config/skills/zipai/SKILL.md
```

The IDE will list `zipai-optimizer` as an available skill and apply the rules automatically.

### Claude Code CLI

```bash
cp CLAUDE.md /path/to/your/project/CLAUDE.md
```

Claude Code reads this file at startup and applies the rules for the workspace session.

### VS Code Agent Extensions (Cline, Roo Code, etc.)

```bash
mkdir -p /path/to/your/project/.agents/rules/
cp .agents/rules/zipai.md /path/to/your/project/.agents/rules/zipai.md
```

### Gemini CLI

```bash
cp SKILL.md /path/to/your/project/GEMINI.md
```

### Hermes Agent & Custom AI Assistants

These rules are framework-agnostic markdown. Load `SKILL.md` into any system instructions template:
- **Hermes Agent:** Append to the agent's system instructions profile or `config.yaml`.
- **Custom loops:** Reference `SKILL.md` directly in system instructions.

---

## File Structure

```
ZipAI/
├── SKILL.md                    # Canonical rules (v15.0) — single source of truth
├── CLAUDE.md                   # Mirror for Claude Code auto-detection
├── .agents/rules/zipai.md     # Mirror for VS Code agent extensions
├── README.md                   # This file
├── LICENSE                     # MIT License
├── .pre-commit-config.yaml    # Git hooks
├── .gitattributes             # Git merge strategies
└── .github/
    ├── scripts/
    │   └── ai-pr-reviewer.cjs # AI PR review script (Gemini-powered)
    └── workflows/
        ├── ai-pr-reviewer.yml           # Auto-review workflow
        └── auto-resolve-pr-conflicts.yml # Auto-rebase open PRs
```

> **Note:** `CLAUDE.md` and `.agents/rules/zipai.md` are mirrors of `SKILL.md`. When updating the rules, update `SKILL.md` first, then propagate.

---

## Versioning

| Version | Changes |
|---|---|
| **15.0** | Added sections 8-12: Schema Enforcement, Semantic Input Compression, ARM-style Reasoning, KV Cache Optimization, TAAC. Expanded Limitations. |
| **14.0** | Initial public release. Rules 1-7: Zero Filler, Ambiguity, Prompt Caching, Input Pruning, Surgical Output, Grammar Density, Reasoning Budget. |

---

## Limitations

- **Brainstorming:** Disable during creative/open-ended design phases.
- **Grep Blindness:** Key context may fall outside filter boundaries.
- **Overshadowing:** Aggressive pruning may drop micro-variables in long sessions.
- **Math Fragility:** Numerical reasoning chains resist aggressive compression.

---

## License

[MIT](./LICENSE)
