<p align="center">
  <a href="https://github.com/qi-xiao-bai/salesforce-develop-skill/blob/main/LICENSE.txt">
    <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="License: MIT">
  </a>
  <a href="https://github.com/qi-xiao-bai/salesforce-develop-skill">
    <img src="https://img.shields.io/badge/Codex-Skill-blue" alt="Codex Skill">
  </a>
  <a href="https://github.com/qi-xiao-bai/salesforce-develop-skill">
    <img src="https://img.shields.io/badge/Salesforce-Apex-1798c1" alt="Salesforce Apex">
  </a>
  <a href="https://github.com/qi-xiao-bai/salesforce-develop-skill">
    <img src="https://img.shields.io/badge/Workflow-Documentation--First-6f42c1" alt="Documentation First">
  </a>
  <a href="https://github.com/qi-xiao-bai/salesforce-develop-skill">
    <img src="https://img.shields.io/badge/Backup-Controlled-orange" alt="Controlled Backup">
  </a>
  <a href="https://github.com/qi-xiao-bai/salesforce-develop-skill">
    <img src="https://img.shields.io/badge/sfdx-Forbidden-critical" alt="No sfdx">
  </a>
</p>

# Salesforce Develop Skill

<p align="center">
  A documentation-first Codex skill for disciplined Salesforce development workflows.<br>
  一个面向规范化 Salesforce 开发流程、以文档优先为核心的 Codex Skill。
</p>

<p align="center">
  Built for teams and solo developers who want Salesforce changes to stay aligned with task documents, reviewable implementation flows, and controlled delivery boundaries.<br>
  适用于希望将 Salesforce 代码改动始终保持在任务文档、可审查实现链路与受控交付边界之内的团队与个人开发者。
</p>

[中文介绍](#中文介绍)

## English

### Overview

Salesforce Develop is a specialized Codex skill for Salesforce development workflows where implementation quality depends not only on code generation, but also on process discipline.

It is built for scenarios where a task-specific development document exists, code changes must align with an existing implementation plan, comments and debug statements should follow team conventions, and environment actions such as `sfdx` pull or deploy must be strictly prohibited.

Rather than behaving like a generic coding assistant, this skill behaves like a structured implementation partner.

### Why This Skill Exists

Salesforce projects often become difficult to maintain when development work loses procedural consistency. Common problems include:

- editing code before reading the active task document
- changing logic without preserving the intended workflow
- missing or noisy comments and debug statements
- implementation drifting away from development notes
- unsafe environment commands being executed too early

This skill turns those requirements into explicit operating rules.

### Core Capabilities

- Requires reading the matching project-root `*-开发文档.md` before any edits
- Preserves user-owned development documents and updates them only on explicit request
- Enforces Chinese comments for Salesforce code changes
- Requires class-level and method-level comments for modified code
- Encourages meaningful `System.debug` placement without redundant duplicate logs
- Supports controlled source backups under the project-root `备份` directory
- Forbids all `sfdx` command execution
- Provides both Chinese and English development document templates for reuse

### Target Users

- Developers maintaining Salesforce business systems with strict process expectations
- Teams that use task-specific development documents as the working contract
- Projects where traceability, readability, and safe edits matter more than one-shot generation speed
- Users who want Codex to behave like a disciplined implementation assistant

### Repository Layout

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

### File Guide

#### `SKILL.md`

The default public skill entry file. This is the primary skill file used by Codex.

#### `SKILL_CN.md`

The Chinese version of the skill rules, intended for Chinese-speaking users and workflows where Chinese wording is the natural default.

#### `references/development-doc-template.md`

The English public template for maintaining current-implementation documents.

#### `references/开发文档模板.md`

The Chinese version of the implementation-document template.

#### `scripts/backup_source_file.py`

A helper script for backing up a source file into the project-root `备份` directory while keeping at most three backups per source file.

### Operating Principles

This skill follows a few strict principles:

1. Understand the current implementation before editing code.
2. Respect the active development document as the task contract.
3. Keep comments useful and code-focused.
4. Add debug logs for observability, not noise.
5. Prefer minimal, controlled changes over broad refactors.
6. Never mix development assistance with deployment actions.

### Typical Workflow

1. Identify the project root.
2. Find the most relevant `*-开发文档.md`.
3. Read it before making any edits.
4. Extract current progress, confirmed rules, and expected outcomes.
5. Modify only the relevant code path.
6. Add Chinese comments and targeted debug statements.
7. Update the development document only when explicitly requested.

### Backup Strategy

When the user explicitly requests a backup, the skill uses a fixed convention:

- backup location: project-root `备份`
- naming: `original-file-name-timestamp.ext`
- retention: keep at most three backups per source file
- rollover policy: delete the oldest backup before creating the fourth one

### Language Strategy

This public repository uses English as the default publishing language while preserving Chinese workflow assets for the actual development style the skill was designed for.

That means:

- public-facing materials remain accessible to broader audiences
- Chinese code-comment conventions remain intact
- Chinese development-document templates remain first-class resources

### Installation

Copy this folder into your Codex skills directory, typically:

```text
$CODEX_HOME/skills/salesforce-develop
```

or on a default Windows setup:

```text
%USERPROFILE%\\.codex\\skills\\salesforce-develop
```

### Suggested Invocation

Use:

```text
Use $salesforce-develop to work on this Salesforce task.
```

For stricter behavior:

```text
Use $salesforce-develop, read the matching development document first, do not touch it unless asked, and follow the backup and Chinese comment rules.
```

### License

- Legal text: [LICENSE.txt](LICENSE.txt)
- Chinese reference version: [LICENSE_CN.txt](LICENSE_CN.txt)

---

## 中文介绍

### 项目概述

Salesforce Develop 是一个面向 Salesforce 开发场景的 Codex Skill，重点不只是“帮你写代码”，而是让整个开发过程更有规约、更可追踪、也更适合真实业务项目维护。

它适用于这类场景：每个任务都对应一个开发文档，代码修改必须对齐当前实现思路，注释和 debug 需要统一风格，并且必须严格禁止 `sfdx` 拉取、部署等环境命令。

与通用型代码助手不同，这个 skill 的定位更接近“有开发规约意识的实现协作助手”。

### 为什么需要这个 Skill

很多 Salesforce 项目后期难维护，并不是因为代码不会写，而是因为开发过程失去了统一约束。常见问题包括：

- 还没读任务开发文档就开始改代码
- 改了逻辑但没有保留原有链路和设计意图
- 注释和 debug 要么缺失，要么噪音过多
- 实现逐渐偏离开发文档
- 过早执行拉取、部署、同步环境等危险操作

这个 skill 的价值，就是把这些要求固化成明确规则。

### 核心能力

- 强制要求改代码前先读取项目根目录下匹配的 `*-开发文档.md`
- 默认不自动维护开发文档，只有用户明确要求时才允许更新
- 约束 Salesforce 代码使用中文注释
- 要求新增或修改过的类、方法必须有中文注释
- 鼓励在关键逻辑处添加有信息量的 `System.debug`
- 支持按规约备份源文件到项目根目录 `备份` 文件夹
- 严格禁止执行任何 `sfdx` 命令
- 同时提供中英文开发文档模板，方便复用和公开发布

### 适用对象

- 维护 Salesforce 业务系统、对开发流程有明确要求的开发者
- 以任务开发文档作为工作依据的团队
- 比起“一次性生成代码”，更重视可读性、可维护性和改动可控性的项目
- 希望 Codex 像一个遵守规约的实现伙伴，而不是通用助手的用户

### 仓库结构

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

### 文件说明

#### `SKILL.md`

英文主 skill 文件，也是公开发布版本默认使用的主入口。

#### `SKILL_CN.md`

中文规则版 skill 文件，适合以中文为主的开发习惯和中文团队协作场景。

#### `references/development-doc-template.md`

英文版开发文档模板，适合公开发布和英文使用者参考。

#### `references/开发文档模板.md`

中文版开发文档模板，适合中文项目和中文维护场景。

#### `scripts/backup_source_file.py`

备份脚本，用于把源文件备份到项目根目录下的 `备份` 文件夹，并自动控制同一文件最多保留三份备份。

### 工作原则

这个 skill 的核心原则是：

1. 改代码前先理解当前实现
2. 把开发文档视为当前任务的工作契约
3. 注释要有意义，不写流水账
4. debug 要帮助观察，不制造噪音
5. 优先做最小、可控的改动
6. 严格把开发协助和环境部署行为分开

### 典型工作流

1. 识别项目根目录
2. 找到最匹配当前任务的 `*-开发文档.md`
3. 先阅读文档，再改代码
4. 提取当前进度、已确认规则和预期结果
5. 只修改当前链路真正相关的代码
6. 补中文注释和关键 debug
7. 只有在明确要求时才维护开发文档

### 备份策略

当用户明确要求备份文件时，skill 采用固定规则：

- 备份位置：项目根目录 `备份`
- 命名方式：`原文件名-时间戳.扩展名`
- 保留策略：同一个源文件最多保留三份
- 轮转规则：第 4 次备份时先删除最老的一份

### 语言策略

这个公开仓库默认使用英文做发布语言，但同时完整保留中文工作流资产。

这意味着：

- 对外公开展示时更容易被更广泛用户理解
- 中文注释和中文开发习惯不需要牺牲
- 中文开发文档模板和中文规则仍然是这个 skill 的核心组成部分

### 安装方式

把整个文件夹复制到 Codex 的 skills 目录即可，常见路径例如：

```text
$CODEX_HOME/skills/salesforce-develop
```

或者 Windows 默认环境下：

```text
%USERPROFILE%\\.codex\\skills\\salesforce-develop
```

### 推荐调用方式

可以这样用：

```text
Use $salesforce-develop to work on this Salesforce task.
```

如果你希望它更严格遵守规约，也可以这样写：

```text
Use $salesforce-develop, read the matching development document first, do not touch it unless asked, and follow the backup and Chinese comment rules.
```

### 许可证

- 英文原文：[LICENSE.txt](LICENSE.txt)
- 中文参考版：[LICENSE_CN.txt](LICENSE_CN.txt)
