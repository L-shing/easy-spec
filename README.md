# easy-spec

[中文](README.md) | [English](README.en.md)

面向 Agent 的轻量规格驱动流程：在写代码前，用四份可评审文档把门禁走完。

**技术方案 → DDL → API → 开放问题**

适用于中大型需求（跨表、跨接口、跨模块）。纯 bugfix / 小改动不必启用。

## Features

- **四步门禁**：每一步需人工确认后再进入下一步
- **四份产物**：技术方案、DDL、前端 API、开放问题，结构固定、可 Review
- **疑问后置**：开发中先记入开放问题，收尾统一确认，不打断主流程
- **多工具可用**：Cursor / Claude Code / Codex 及其它支持 Skills 的 Agent

## Installation

将本仓库完整拷贝到对应工具的 skills 目录，并保留目录名 `easy-spec`：

```bash
# Cursor（个人）
cp -R easy-spec ~/.cursor/skills/easy-spec

# Cursor（项目内）
cp -R easy-spec <repo>/.cursor/skills/easy-spec

# Claude Code
cp -R easy-spec ~/.claude/skills/easy-spec

# Codex
cp -R easy-spec ~/.codex/skills/easy-spec
```

无 Skills 机制时，可将 `SKILL.md` 写入项目的 `AGENTS.md` / `CLAUDE.md` / `.github/copilot-instructions.md`，并把 `templates/` 一并纳入仓库。

## Quick Start

```text
这是 PRD：…
我的分工：…
按 easy-spec 走，先出技术方案。
```

门禁口令：`方案 OK` → `DDL OK` → `API OK` → `开始开发`  
收尾：`把疑问统一问我`

也可说：`四步四文档` / `先出方案再写代码`，或在 Cursor 中 `@easy-spec`。

## Workflow

| Step | Output | Gate |
|------|--------|------|
| 1 | `01-tech-design` | `方案 OK` |
| 2 | `02-ddl` | `DDL OK` |
| 3 | `03-api` | `API OK` |
| 4 | 实现 + `04-open-questions` | 收尾统一确认疑问 |

复杂链路优先使用 Mermaid，示例见 [`examples/mermaid-flowchart-demo.md`](examples/mermaid-flowchart-demo.md)。

## Interop

easy-spec 聚焦「先把规格门禁走完」。门禁通过后的文档（技术方案、DDL、API、开放问题）可直接交给 [Superpowers](https://github.com/obra/superpowers)、[OpenSpec](https://github.com/Fission-AI/OpenSpec) 等开源 SDD / spec 工作流，继续做实现、拆任务与落地开发——本仓库产出的是可交接的上游规格，而不是与下游流程互斥的另一套体系。

## Project Structure

```text
easy-spec/
├── SKILL.md              # Agent 指令
├── templates/            # 四份文档模版
│   ├── 01-tech-design.md
│   ├── 02-ddl.md
│   ├── 03-api.md
│   └── 04-open-questions.md
├── examples/             # 示例
├── CHANGELOG.md
└── LICENSE
```

## License

[MIT](LICENSE) © LC
