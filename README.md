# Salesforce Develop Skill

An opinionated Codex skill for Salesforce engineering work that prioritizes implementation discipline over ad-hoc code generation.

This skill is designed for teams and individual developers who want Salesforce development tasks to follow a predictable workflow: read the active development document first, preserve user-owned design intent, write Chinese code comments, add meaningful debug checkpoints, and avoid unsafe environment actions such as `sfdx` pull or deploy commands.

## Why This Skill Exists

Salesforce work often fails not because the model cannot write Apex, but because the development process becomes inconsistent:

- code is edited before the current task document is understood
- logic is changed without preserving the intended workflow
- comments and debug statements are either missing or noisy
- documentation drifts away from implementation
- deployment commands are executed too early

This skill turns those constraints into an explicit development protocol.

## Core Capabilities

- Enforce a mandatory pre-read of the matching project-root `*-开发文档.md` before any code edits
- Preserve user-owned development documents and update them only on explicit request
- Require Chinese comments for Salesforce code changes
- Require method-level and class-level comments for modified code
- Encourage meaningful `System.debug` placement without redundant duplicate logs
- Support controlled source backups under the project-root `备份` directory
- Forbid any `sfdx` command execution
- Provide both Chinese and English development document templates for public reuse

## Who This Is For

- Developers maintaining Salesforce business systems with strong process requirements
- Teams that rely on task-specific development documents as the source of truth
- Projects where traceability, readability, and controlled edits matter more than fast one-shot generation
- Users who want a Codex skill to behave like a disciplined implementation partner rather than a generic coding assistant

## Repository Layout

```text
salesforce-develop/
├── SKILL.md
├── SKILL_CN.md
├── README.md
├── LICENSE.txt
├── LICENSE_CN.txt
├── .gitignore
├── agents/
│   └── openai.yaml
├── references/
│   ├── development-doc-template.md
│   └── 开发文档模板.md
└── scripts/
    └── backup_source_file.py
```

## File Guide

### `SKILL.md`

The default public entry file. This is the primary skill file used by Codex.

### `SKILL_CN.md`

The Chinese version of the skill rules, intended for Chinese-speaking users and workflows where Chinese wording is the natural default.

### `references/development-doc-template.md`

The English public template for maintaining current-implementation documents.

### `references/开发文档模板.md`

The Chinese version of the implementation-document template.

### `scripts/backup_source_file.py`

Helper script for backing up a source file into the project-root `备份` directory while keeping at most three backups per source file.

## Operating Principles

This skill follows a few strict principles:

1. Understand the current implementation before editing code.
2. Respect the user’s active development document as the task contract.
3. Keep comments useful and code-focused.
4. Add debug logs for observability, not noise.
5. Prefer minimal, controlled changes over broad refactors.
6. Never mix development assistance with environment deployment actions.

## Typical Workflow

1. Identify the project root.
2. Find the most relevant `*-开发文档.md`.
3. Read it before making any edits.
4. Extract current progress, confirmed rules, and expected outcomes.
5. Modify only the relevant code path.
6. Add Chinese comments and targeted debug statements.
7. Update the development document only when the user explicitly asks for it.

## Backup Strategy

When the user explicitly requests a backup, the skill uses a fixed convention:

- backup location: project-root `备份`
- naming: `original-file-name-timestamp.ext`
- retention: keep at most three backups per source file
- rollover policy: delete the oldest backup before creating the fourth one

## Notes on Language

The public repository uses English as the default publishing language, while preserving Chinese workflow assets for the actual development style this skill was designed for.

That means:

- public-facing structure can be understood by broader audiences
- Chinese code-comment conventions remain intact
- Chinese development-document templates remain first-class resources

## Installation

Copy this folder into your Codex skills directory, typically:

```text
$CODEX_HOME/skills/salesforce-develop
```

or on a default Windows setup:

```text
%USERPROFILE%\\.codex\\skills\\salesforce-develop
```

## Suggested Invocation

Use:

```text
Use $salesforce-develop to work on this Salesforce task.
```

If you want stricter behavior, specify:

```text
Use $salesforce-develop, read the matching development document first, do not touch it unless asked, and follow the backup and Chinese comment rules.
```

## Chinese Summary / 中文简介

这是一个面向 Salesforce 开发场景的 Codex Skill，核心目标不是“快速生成代码”，而是“按规约稳妥开发”。

它特别适合下面这种工作方式：

- 每个任务都有对应的开发文档
- 改代码前必须先读文档
- 代码注释和 debug 需要统一风格
- 需要保留原始代码备份
- 严格禁止直接执行 `sfdx` 拉取、部署等环境命令

如果你更习惯中文规则说明，请直接阅读：

- [SKILL_CN.md](SKILL_CN.md)
- [references/开发文档模板.md](references/开发文档模板.md)

## License

- Legal text: [LICENSE.txt](LICENSE.txt)
- Chinese reference version: [LICENSE_CN.txt](LICENSE_CN.txt)
