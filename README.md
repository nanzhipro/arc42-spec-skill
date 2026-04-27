# arc42-spec-skill

> 把一句开发需求，整理成一份能讨论、能评审、能继续实现的 arc42 中文技术方案。

[![Skill](https://img.shields.io/badge/skill-arc42--spec-blue)](skills/arc42-spec/SKILL.md)
[![arc42](https://img.shields.io/badge/framework-arc42-green)](arc42-architecture.md)
![Language](https://img.shields.io/badge/language-zh--CN-lightgrey)

不是每个团队都缺“会写代码的人”。

很多时候，真正缺的是一份在动手前就把目标、边界、约束、风险和取舍说清楚的方案。AI 让编码更快了，但如果需求表达还是模糊的，返工、争论和回滚也会一样快。`arc42-spec` 这个 skill，专门用来补上这一步。

## 这是什么

`arc42-spec` 是一个给 AI Agent 使用的 skill。

当你说出类似下面的话：

- “为 OAuth 登录改造写技术方案”
- “为订单模块重构出一份 arc42 spec”
- “为一次迁移 / 集成写设计文档”

Agent 会把需求转写成一份符合 arc42 12 章结构的中文方案文档，作为后续实现、评审和交接的共同依据。

你不需要先懂 arc42，也不需要先会写“标准架构文档”。你只需要把问题讲清楚，这个 skill 负责把问题组织成一份可执行、可审阅、可追溯的文档。

## 为什么它对普通开发者也有用

很多项目出问题，并不是因为没人能写代码，而是因为下面这些问题在写代码之前没有被说清楚：

- 这次到底要解决什么问题，什么不在范围内
- 方案依赖哪些现有约束，哪些地方不能随便改
- 为什么选 A 不选 B，以后谁来接手都能看懂吗
- 当前风险是什么，验证方式是什么，回退路径是什么

`arc42-spec` 的价值，不是“多生成一份文档”，而是把这些原本会在评审会、返工和线上故障里暴露出来的问题，尽量提前到方案阶段解决。

## 你会拿到什么

一份合格的输出，通常会覆盖这些内容：

- 为什么现在要做这件事
- 这次明确要达成什么，以及明确不做什么
- 系统边界、外部依赖和关键约束
- 顶层方案、模块拆分、关键流程和部署方式
- 核心架构决策，以及为什么这样选
- 质量要求、风险、技术债和后续验证方式

如果你想先看成品长什么样，可以直接看示例：[examples/sample-spec-oauth.md](examples/sample-spec-oauth.md)。

## 它适合哪些场景

这类场景最适合先跑一版 `arc42-spec`：

- 新功能设计
- 旧模块重构
- 第三方系统集成
- 迁移、升级与架构演进
- 复杂 bug 的修复方案设计

如果只是改一个文案、修一个很小的局部问题，通常没必要上完整的 arc42 方案。

## 3 分钟上手

### 1. 安装 skill

使用 [skills](https://www.npmjs.com/package/skills) 安装：

```bash
# 用户级安装：在所有项目可用
npx -y skills add nanzhipro/arc42-spec-skill

# 项目级安装：仅当前仓库可用
npx -y skills add nanzhipro/arc42-spec-skill --project
```

更新到最新版本：

```bash
npx -y skills add nanzhipro/arc42-spec-skill --force
```

### 2. 在你的项目里直接提需求

注册完成后，在支持 Skill 协议的 AI Agent 中直接说：

- “为支付回调重构写技术方案”
- “为 OAuth 登录接入生成 arc42 文档”
- “为这次数据库迁移出一份 spec”

### 3. 让 Agent 生成方案文档

skill 会按协议读取必要上下文，生成一份中文 Markdown 文档。默认会优先写入仓库里已有的 `docs/specs`、`docs/design`、`docs/rfcs` 或 `docs/arc42` 目录；如果这些目录都不存在，再按约定创建目标路径。

## 这个仓库里有什么

如果你想继续深入，建议按这个顺序阅读：

1. 先读这份 README，判断它解决的问题是不是你正在遇到的那个问题。
2. 看示例输出：[examples/sample-spec-oauth.md](examples/sample-spec-oauth.md)。
3. 看执行协议：[skills/arc42-spec/SKILL.md](skills/arc42-spec/SKILL.md)。
4. 看框架背景：[arc42-architecture.md](arc42-architecture.md)。

仓库中的核心内容分别是：

- [skills/arc42-spec/SKILL.md](skills/arc42-spec/SKILL.md)：真正交给 Agent 执行的协议
- [arc42-architecture.md](arc42-architecture.md)：对 arc42 背景、结构和原则的解释
- [examples/sample-spec-oauth.md](examples/sample-spec-oauth.md)：一份可直接参考的完整样例

## 这个项目的立场

这个仓库并不想把技术方案写成“好看的长文”，而是坚持三件事：

- **先说清楚，再开始写代码**
- **先取证，再下结论**
- **先记录取舍，再谈后续演进**

所以它不是一个“写作模板”，而是一个更偏工程实践的协议化 skill。

## 如果你还不熟悉 arc42

也没关系。你可以先把它理解成一套很成熟的问题清单：

- 目标是什么
- 边界在哪里
- 方案怎么做
- 风险是什么
- 为什么这么选

`arc42-spec` 做的，就是把这套问题清单变成 AI 能稳定执行的一次工作流。

## 为什么选择 arc42

[arc42](https://arc42.org/) 是一套非常成熟的架构沟通框架。对普通开发者来说，你不用先记住 12 个章节名，只要先知道它解决了两个常见问题：

- 技术方案终于有了稳定结构，不会每个人各写一套
- 讨论重点不再停留在“感觉这样写也行”，而是回到目标、约束、决策和风险

如果你想进一步理解它的完整背景，可以看：[arc42-architecture.md](arc42-architecture.md)。

## 延伸阅读

- 想看完整执行协议：[skills/arc42-spec/SKILL.md](skills/arc42-spec/SKILL.md)
- 想看一份完整样例：[examples/sample-spec-oauth.md](examples/sample-spec-oauth.md)
- 想理解 arc42 的框架背景：[arc42-architecture.md](arc42-architecture.md)
