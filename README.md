# App Competition Skills

一组面向 AI Agent 的应用竞品分析 skills，用来把“找竞品、筛竞品、写竞品报告”整理成可复用、可追踪、可扩展的工作流。

这个仓库适合以下场景：

- 想给项目建立长期竞品跟踪机制
- 想把新增竞品发现流程标准化
- 想稳定产出带证据链的竞品分析报告
- 想把竞品分析能力沉淀成团队可复用的 skills

## 仓库包含什么

仓库当前包含 3 个核心 skill：

- `app-competition-core`
- `app-competition-discovery`
- `app-competition-reporting`

它们的职责拆分如下：

- `app-competition-core`：负责初始化竞品分析工作区、维护项目级配置，并把任务路由到 discovery 或 reporting。
- `app-competition-discovery`：负责发现新增竞品、生成候选审核清单，并给出 `Track / Watch / Drop` 决策。
- `app-competition-reporting`：负责生成深度竞品分析报告、结构化 JSON 快照，以及项目内的回写更新。

## 工作流概览

这组 skills 的目标不是做一次性的“竞品调研”，而是建立一个可持续运行的竞品分析机制。

典型流程如下：

1. 用 `app-competition-core` 初始化项目内的竞品分析目录和基础文件。
2. 用 `app-competition-discovery` 从目标 app、关键词、相似应用和榜单中发现新的候选竞品。
3. 按评分和证据自动将候选归类为 `Track`、`Watch` 或 `Drop`。
4. 用 `app-competition-reporting` 对重点竞品产出 Markdown 报告和 JSON 快照。
5. 持续维护 `focus-apps`、报告历史和最近快照，形成长期跟踪闭环。

## 仓库结构

```text
.
├── README.md
├── .gitattributes
├── .gitignore
└── skills/
    ├── app-competition-core/
    ├── app-competition-discovery/
    └── app-competition-reporting/
```

每个 skill 目录下通常包含以下资源：

- `SKILL.md`：skill 主说明文档
- `agents/`：agent 配置
- `assets/`：模板文件，如报告模板、快照模板、项目初始化模板
- `references/`：规则说明、字段约定、证据规范、MCP 使用约定
- `examples/`：典型使用示例

## 如何使用

把仓库中的 `skills/app-competition-*` 目录复制到你的技能目录中即可使用。

在实际项目里，建议配套维护以下竞品分析工作区：

- `docs/app-competition/app-compete.md`
- `docs/app-competition/focus-apps.md`
- `docs/app-competition/reports/`
- `docs/app-competition/snapshots/`
- `docs/app-competition/evidence/`

## 这组 skills 的设计原则

- `default_market` 必须显式声明，避免分析市场不清导致结论失真
- 任何“关键词波动、版本变化、素材变化、评论趋势”判断都应带证据
- discovery 输出的是“发现与审核结果”，不是伪装成深度报告的数据堆砌
- reporting 必须同时产出人读版 Markdown 和机器读版 JSON
- 不把所有候选都塞进长期重点列表，而是通过规则控制跟踪范围

## 适合继续扩展的方向

如果后续你要继续扩展这个仓库，比较自然的方向有：

- 增加面向不同市场的配置模板
- 增加自动化周报或定时巡检能力
- 增加关键词、评论、定价变化的统一快照结构
- 增加更多 examples，方便团队成员快速上手

## 说明

这个仓库已经按独立 Git 仓库组织，可以直接作为远程 GitHub 仓库维护和迭代。
