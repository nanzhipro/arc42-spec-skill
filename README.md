# arc42-framework-skill

> 把任意开发需求（新功能 / 重构 / 集成 / 迁移 / 演进 / 修复方案）转写为符合 **arc42** 12 章结构的中文技术方案文档，作为下游 AI 实现与人类评审的共同契约。

[![Skill](https://img.shields.io/badge/skill-arc42--spec-blue)](skills/arc42-spec/SKILL.md)
[![arc42](https://img.shields.io/badge/framework-arc42-green)](arc42-architecture.md)
[![Language](https://img.shields.io/badge/language-zh--CN-lightgrey)](#)

---

## 目录

- [项目定位](#项目定位)
- [架构分层](#架构分层)
- [安装](#安装)
- [快速上手](#快速上手)
- [使用场景](#使用场景)
- [文档导航](#文档导航)
- [设计取舍](#设计取舍)

---

## 项目定位

一份合格的技术方案应当回答 **目标 / 边界 / 结构 / 流程 / 部署 / 质量 / 决策 / 风险** 八类问题。本仓库提供：

1. **arc42 12 章框架**的通用规约 —— 回答“架构表达应该有哪些稳定位置”。
2. **契约式 skill** —— 让 AI 不再凭印象写散文，而是按 Phase / MANDATORY / FORBIDDEN / EXIT 门禁逐章生成。
3. **可参照的样例文档** —— 验证 skill 在真实变更类型上的产出形态。

> 本仓库不是写作风格指南，是**协议**：每一章有可校验的退出条件，跨章有一致性断言，失败有显式回退。

---

## 架构分层

由抽象到落地，自下而上分三层。每一层独立可读，上层依赖下层。

```text
┌─────────────────────────────────────────────────────┐
│  L3  样例层      examples/sample-spec-oauth.md      │  ← 验证产物
├─────────────────────────────────────────────────────┤
│  L2  契约层      skills/arc42-spec/SKILL.md         │  ← 执行协议
├─────────────────────────────────────────────────────┤
│  L1  框架层      arc42-architecture.md              │  ← 通用规约
└─────────────────────────────────────────────────────┘
```

| 层 | 职责 | 文件 | 何时阅读 |
| --- | --- | --- | --- |
| **L1 框架层** | 阐释 arc42 的 12 章结构、相关方驱动、质量目标驱动、多视图分离、决策可追溯等核心思想 | [`arc42-architecture.md`](arc42-architecture.md) | 想理解“为什么 arc42 这样组织”时 |
| **L2 契约层** | 把框架转译为 AI 可执行的协议：Phase A–G、章节契约、跨章断言、失败回退 | [`skills/arc42-spec/SKILL.md`](skills/arc42-spec/SKILL.md) | 想让 AI 生成或评审 spec 时 |
| **L3 样例层** | 在合成 `context_pack` 上演示 skill 的完整产出 | [`examples/sample-spec-oauth.md`](examples/sample-spec-oauth.md) | 想看“合格 spec 长什么样”时 |

---

## 安装

按使用环境三选一。所有方式安装的都是同一个 [`skills/arc42-spec/`](skills/arc42-spec/) 目录。

### 方式 A：`npx` 一键拉取（推荐试用）

用 [`degit`](https://github.com/Rich-Harris/degit) 把 skill 目录抓到 Agent 的 skill 加载路径，无需 clone 整个仓库、无 git 历史包袱。

```bash
# Claude Code（用户级 skill）
npx -y degit cyberserval/arc42-framework-skill/skills/arc42-spec \
  ~/.claude/skills/arc42-spec

# 通用 Agent skill 目录（Codex / 自建 Agent）
npx -y degit cyberserval/arc42-framework-skill/skills/arc42-spec \
  ~/.agents/skills/arc42-spec

# 项目级 skill（仅当前仓库可用）
npx -y degit cyberserval/arc42-framework-skill/skills/arc42-spec \
  .claude/skills/arc42-spec
```

升级：重跑同一条命令并加 `--force`。

```bash
npx -y degit --force cyberserval/arc42-framework-skill/skills/arc42-spec \
  ~/.claude/skills/arc42-spec
```

### 方式 B：Claude Code Plugin / Marketplace

把仓库注册为 Claude Code 的 plugin marketplace，再通过 `/plugin` 安装、升级、卸载。

```text
# 在 Claude Code 里依次执行
/plugin marketplace add cyberserval/arc42-framework-skill
/plugin install arc42-spec
```

常用管理命令：

| 操作 | 命令 |
| --- | --- |
| 查看已装插件 | `/plugin list` |
| 升级到最新版 | `/plugin update arc42-spec` |
| 卸载 | `/plugin uninstall arc42-spec` |
| 移除 marketplace | `/plugin marketplace remove cyberserval/arc42-framework-skill` |

> 启用后在任意会话直接说“写技术方案 / 出 arc42 / 写 spec”即可触发；无需手动 `/load`。

### 方式 C：手动 clone 或 symlink（开发者）

适合需要本地修改 skill 协议或参与贡献。

```bash
git clone https://github.com/cyberserval/arc42-framework-skill.git
cd arc42-framework-skill

# 软链到任一 Agent 的 skill 目录
ln -s "$PWD/skills/arc42-spec" ~/.claude/skills/arc42-spec
```

### 安装位置一览

| Agent | 用户级路径 | 项目级路径 |
| --- | --- | --- |
| Claude Code | `~/.claude/skills/arc42-spec/` | `.claude/skills/arc42-spec/` |
| 通用 Agent / Codex | `~/.agents/skills/arc42-spec/` | `.agents/skills/arc42-spec/` |
| GitHub Copilot Chat | 在仓库 `.github/copilot-instructions.md` 中引用 `SKILL.md` | — |

> 验证安装成功：在 Agent 中说“列出可用的 skill”或直接发起“为 X 写技术方案”，Agent 应能加载 `arc42-spec`。

---

## 快速上手

### 前置条件

- 已通过 [安装](#安装) 任一方式注册 skill。
- 使用支持 Skill 协议的 AI Agent（Claude Code、GitHub Copilot Chat、Codex CLI 等）。

### 三步触发

```text
1. 在仓库根目录唤起 AI Agent
2. 提出需求："为 X 写技术方案" / "出 arc42 / 写 RFC / 写 spec"
3. AI 按 SKILL.md 协议执行 Phase A→G，写出文档到 docs/specs/<slug>.md
```

### 输入契约（精简版）

| 字段 | 必填 | 说明 |
| --- | :---: | --- |
| `requirement` | ✅ | 自然语言描述要解决什么问题 |
| `output_path` | ✱ | 默认按仓库已有 `docs/specs|design|rfcs|arc42/` 顺序选取 |
| `scope_level` | ✱ | `S` / `M` / `L`，默认按决策表自动判定 |
| `language` | ✱ | 默认跟随 `requirement` 语言 |

> 详细契约、AskUserQuestion 触发条件、命名规则见 [`SKILL.md` §1 输入契约](skills/arc42-spec/SKILL.md#1-输入契约)。

---

## 使用场景

按变更类型选择使用方式。skill 内置六类变更的上下文发现策略。

| 变更类型 | 触发关键词 | 必读上下文 |
| --- | --- | --- |
| `new-feature` | 新功能 / 新模块 | README、目标模块入口、相邻已存在功能 |
| `refactor` | 重构 / 改造 | 被改动模块、调用方、被调方、相关测试 |
| `integration` | 集成 / 对接 | 集成目标的现有接口、数据模型、既有适配器 |
| `migration` | 迁移 / 升级 | 旧实现、新实现目标、迁移文档、数据样本 |
| `evolution` | 架构演进 | 现有架构文档、近 30 commits |
| `bugfix-design` | 修复方案设计 | 复现路径、错误日志、相关代码与测试 |

> 各类型的完整上下文清单见 [`SKILL.md` §2 Phase C](skills/arc42-spec/SKILL.md#phase-c--上下文发现)。

---

## 文档导航

### 按问题查阅

| 想知道… | 去哪 |
| --- | --- |
| 12 章每章应该写什么 | [`arc42-architecture.md` §3 12 章结构](arc42-architecture.md) |
| 每章的 MANDATORY / FORBIDDEN / EXIT | [`SKILL.md` §3 章节契约](skills/arc42-spec/SKILL.md#3-章节契约) |
| 跨章一致性断言 A1–A6 | [`SKILL.md` Phase E](skills/arc42-spec/SKILL.md#phase-e--跨章一致性) |
| 输出文件骨架 | [`SKILL.md` §4 输出骨架](skills/arc42-spec/SKILL.md#4-输出骨架) |
| 失败回退策略 | [`SKILL.md` §5 失败回退](skills/arc42-spec/SKILL.md#5-失败回退) |
| 与代码生成的接口约定 | [`SKILL.md` §7 与代码生成的接口](skills/arc42-spec/SKILL.md#7-与代码生成的接口) |
| 一份完整 spec 实例 | [`examples/sample-spec-oauth.md`](examples/sample-spec-oauth.md) |

### 12 章速览

| # | 章节 | 核心问题 |
| :-: | --- | --- |
| 1 | 引言与目标 | 为什么做、目标、非目标、质量目标、相关方 |
| 2 | 架构约束 | 哪些限制不可忽略 |
| 3 | 上下文与范围 | 系统边界在哪里 |
| 4 | 解决方案策略 | 顶层方案如何达成目标 |
| 5 | 构建块视图 | 静态结构如何分解 |
| 6 | 运行时视图 | 关键场景如何发生 |
| 7 | 部署视图 | 软件如何运行和分布 |
| 8 | 横切概念 | 跨模块规则是什么 |
| 9 | 架构决策 | 为什么这样选（ADR） |
| 10 | 质量要求 | 如何证明架构满足质量 |
| 11 | 风险与技术债 | 哪些问题需要管理 |
| 12 | 术语表 | 关键词如何统一 |

> 各章详解见 [`arc42-architecture.md`](arc42-architecture.md)。

---

## 设计取舍

本 skill 与“写作模板”/“风格指南”有本质区别：

- **契约 > 风格**：每章有 EXIT 门禁；不通过即重写或回退，不是“尽量写好”。
- **取证 > 想象**：MANDATORY 字段必须从 `context_pack`（代码 / 配置 / commit / issue）取证；缺失则 `> TODO(...)` 显式标注。
- **跨章断言 > 单章完美**：A1–A6 强制目标—策略—结构—行为—决策—风险闭环。
- **裁剪显式**：S 级允许标 `> 本章不适用`，但**不允许直接删章**。
- **更新模式**：同主题文档已存在 → 追加 ADR / 风险并保留旧决策的 `Supersedes` 链，不覆盖历史。

> 详细规约见 [`SKILL.md` §0 不可绕过的规则](skills/arc42-spec/SKILL.md#不可绕过的规则) 与 [§9 反漂移检查表](skills/arc42-spec/SKILL.md#9-反漂移检查表自检每次执行)。

---

## License

调研报告（[`arc42-architecture.md`](arc42-architecture.md)）基于 arc42 官方公开材料编写；arc42 模板本身遵循其官方许可。本仓库 skill 与样例代码部分以仓库根 `LICENSE` 为准（如未提供，默认保留所有权利）。
