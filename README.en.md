# easy-spec

[中文](README.md) | [English](README.en.md)

A lightweight, agent-oriented spec workflow: ship four reviewable docs before writing code.

**Tech design → DDL → API → Open questions**

Built for mid-to-large features (cross-table, cross-API, cross-module). Skip it for trivial fixes.

## Features

- **Gated steps** — advance only after explicit human approval
- **Four artifacts** — tech design, DDL, frontend API, open questions
- **Deferred questions** — capture blockers while coding; resolve them at the end
- **Tool-agnostic** — Cursor, Claude Code, Codex, and other Skills-capable agents

## Installation

Copy this repository into your tool’s skills directory. Keep the folder name `easy-spec`:

```bash
# Cursor (user-wide)
cp -R easy-spec ~/.cursor/skills/easy-spec

# Cursor (per repo)
cp -R easy-spec <repo>/.cursor/skills/easy-spec

# Claude Code
cp -R easy-spec ~/.claude/skills/easy-spec

# Codex
cp -R easy-spec ~/.codex/skills/easy-spec
```

Without a Skills mechanism, paste `SKILL.md` into `AGENTS.md` / `CLAUDE.md` / `.github/copilot-instructions.md` and keep `templates/` in the repo.

## Quick Start

```text
Here is the PRD: …
My ownership: …
Follow easy-spec; start with the tech design.
```

Gates: `方案 OK` → `DDL OK` → `API OK` → `开始开发`  
Wrap-up: `把疑问统一问我`

Also triggered by: `四步四文档` / `先出方案再写代码`, or `@easy-spec` in Cursor.

## Workflow

| Step | Output | Gate |
|------|--------|------|
| 1 | `01-tech-design` | `方案 OK` |
| 2 | `02-ddl` | `DDL OK` |
| 3 | `03-api` | `API OK` |
| 4 | Implementation + `04-open-questions` | Confirm questions at the end |

Prefer Mermaid for non-trivial flows — see [`examples/mermaid-flowchart-demo.md`](examples/mermaid-flowchart-demo.md).

## Interop

easy-spec stops at gated, reviewable specs. Once approved, the artifacts (tech design, DDL, API, open questions) can be handed off to open-source SDD workflows such as [Superpowers](https://github.com/obra/superpowers) or [OpenSpec](https://github.com/Fission-AI/OpenSpec) for implementation, task breakdown, and delivery. Treat easy-spec as an upstream spec layer — complementary to those tools, not a competing end-to-end pipeline.

## Project Structure

```text
easy-spec/
├── SKILL.md              # Agent instructions
├── templates/            # Four document templates
│   ├── 01-tech-design.md
│   ├── 02-ddl.md
│   ├── 03-api.md
│   └── 04-open-questions.md
├── examples/             # Samples
├── CHANGELOG.md
└── LICENSE
```

## License

[MIT](LICENSE) © LC
