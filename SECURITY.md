# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest release (`SKILL.md` v15.0) | ✅ |
| `main` branch | ✅ |
| Older releases | ❌ |

ZipAI is a fast-moving ruleset; only the latest release and `main` receive updates.

## Reporting a Vulnerability

**Do not** open a public GitHub Issue to report a security problem.

ZipAI is a collection of **markdown prompt-optimization rules** and contains no executable
code, network calls, or secret handling. However, if you believe a rule could:

- cause an agent to leak secrets or sensitive context,
- be abused to bypass safety guardrails of an underlying model/agent, or
- introduce a prompt-injection risk when loaded into a system,

please report it privately:

1. Open a **[GitHub Security Advisory](https://github.com/nickdesi/ZipAI/security/advisories/new)**
   on this repository so the report stays private during triage.
2. Include the affected rule/section, reproduction steps, impact, and any suggested mitigation.

We aim to acknowledge security reports within **72 hours**.

## Scope

- **In scope:** the rules shipped in `SKILL.md`, `CLAUDE.md`, and `.agents/rules/zipai.md`.
- **Out of scope:** third-party agents, models, or tools that consume these rules. Report
  vulnerabilities in those products to their respective maintainers.

## Safe-Usage Best Practices

- Do **not** paste secrets or private keys into prompts while ultra-dense mode is active.
- Disable ZipAI during open-ended/brainstorming phases where full context matters (see Limitations).
- Treat ZipAI output as compressed hints — verify diffs and commands before applying them.
- Keep your agent and its model updated; ZipAI tunes behavior but does not replace vendor safety controls.
