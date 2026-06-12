# ZipAI: Ultra-Dense Token Optimizer

ZipAI is a collection of optimization rules, prompt frameworks, and custom IDE skills designed to minimize LLM input (prefill) and output (generation) token consumption. It focuses on maximizing **Prompt Caching** efficiency, enforcing **Semantic Input Pruning** (inspired by LLMLingua-2), and guiding LLM agents toward ultra-dense, telegraphic interactions.

---

## Key Features

1. **Prompt Caching & Prefix Stability:** Ensures invariant blocks (system instructions, schemas) stay at the top of the prompt while dynamic context is isolated at the bottom, maintaining up to a 90% cache hit rate.
2. **Semantic Log Pruning:** Compresses verbose terminal and test logs by extracting tracebacks and error blocks with a tight 3-5 line context window, reducing log tokens by up to 95%.
3. **AST Skeletal Inspection:** Limits initial file reads by using grep-based structural outlines (signatures, classes, definitions) of large source files before retrieving specific lines.
4. **JSON/YAML Minification:** Enforces key extraction and minification of structured payloads, avoiding useless metadata.
5. **Surgical Output & Localized Diffs:** Constrains generating agents to output only localized diffs (`str_replace`) and telegraphic phrasing, avoiding full-file code reprints (saving up to 90% on generation latency/cost).

---

## Token Savings Benchmark

| Category | Optimization Mechanism | Before | After | Token Reduction |
| :--- | :--- | :--- | :--- | :--- |
| **Log Files** | Traceback extraction + context | ~4,500 tokens | ~225 tokens | **-95.0%** |
| **MCP Payloads** | Minified JSON keys-only | ~1,800 tokens | ~180 tokens | **-90.0%** |
| **Code Inspection** | AST Grep + targeted line view | ~6,200 tokens | ~480 tokens | **-92.2%** |
| **Session Context** | Static-First caching | 30,000 tokens | ~3,000 equivalent tokens | **-90.0%** (via cache) |
| **Agent Generation** | Telegraphic + surgical diffs | ~450 tokens | ~95 tokens | **-78.8%** |

---

## How to Use

### 1. In Antigravity IDE (Gemini Agent Skill)
Copy `SKILL.md` to your custom skills directory:
```bash
mkdir -p ~/.gemini/config/skills/zipai
cp SKILL.md ~/.gemini/config/skills/zipai/SKILL.md
```
The IDE will automatically list `zipai-optimizer` as an available skill. The agent will read it and apply the rules when prompt optimization or token management is required.

### 2. In Claude Code CLI
Copy the rules file to your target project's root as `CLAUDE.md`:
```bash
cp CLAUDE.md /path/to/your/project/CLAUDE.md
```
Claude Code automatically reads this file at startup to guide the agent's behavior throughout the workspace session.

### 3. In VS Code Agent Extensions (Cline, Roo Code, etc.)
Ensure the `.agents/rules/zipai.md` file is present in your workspace root:
```bash
mkdir -p /path/to/your/project/.agents/rules/
cp .agents/rules/zipai.md /path/to/your/project/.agents/rules/zipai.md
```
The agent extension will automatically pick up and adhere to these instructions.

### 4. In OpenCode, Hermes Agent, & Custom AI Assistants
Since these rules are framework-agnostic markdown files, they can be loaded by any custom prompt-based system:
* **Hermes Agent:** Copy the rules directly into the agent's system instructions profile or append them to the configuration files (e.g., under the model prompt configurations in `config.yaml`).
* **OpenCode / Custom Agent loops:** Import or reference `SKILL.md` directly into the system instructions template to force the model to prune input logs, inspect AST structures, and respond in ultra-dense telegraphic format.
