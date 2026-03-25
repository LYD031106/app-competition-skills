---
name: app-competition-core
description: Use when setting up or maintaining project-level app competition tracking, updating docs/app-competition/app-compete.md or focus-apps.md, or routing requests for new competitor discovery and deep competitor reports.
---

# App Competition Core

## Overview

为单个项目维护长期竞品分析工作区。这个 skill 负责初始化 `docs/app-competition/`、维护 `app-compete.md` 与 `focus-apps.md`，并把“找新增 app”与“写深度报告”路由到专用子 skill。

## Quick Start

1. 检查 `docs/app-competition/` 是否存在；缺失时按 [assets/app-compete-template.md](assets/app-compete-template.md) 和 [assets/focus-apps-template.md](assets/focus-apps-template.md) 初始化。
2. 读取 `docs/app-competition/app-compete.md` 与 `docs/app-competition/focus-apps.md`。
3. 如果 `default_market` 未声明，先暂停分析并要求在项目文件里补齐，绝不擅自假设市场。
4. 根据任务类型路由：
   - 初始化目录、更新模板、维护重点问题、更新索引：当前 skill 直接处理。
   - 找新增 app、扩充跟踪池、判断是否纳入观察：使用 `$app-competition-discovery`。
   - 生成竞品深度报告、写 Markdown 和 JSON 快照：使用 `$app-competition-reporting`。
5. 完成后，回写 `app-compete.md` 中的索引、`last_updated`、`report_history` 或候选池说明。

## Routing Guide

| 用户意图 | 处理方式 |
| --- | --- |
| “给这个项目建竞品分析目录” | 当前 skill 初始化文件与目录 |
| “帮我找最近新增的竞品 app” | 路由到 `$app-competition-discovery` |
| “帮我做一份竞品周报/深度报告” | 路由到 `$app-competition-reporting` |
| “把这个 app 加到重点关注列表” | 直接按项目规则更新 `focus-apps.md`，并在报告或变更说明里留痕 |
| “更新重点问题/关键词 watchlist” | 当前 skill 更新 `app-compete.md` |

## Project Contract

项目目录固定为：
- `docs/app-competition/app-compete.md`
- `docs/app-competition/focus-apps.md`
- `docs/app-competition/reports/`
- `docs/app-competition/snapshots/`
- `docs/app-competition/evidence/`

文件职责固定为：
- `app-compete.md`：项目级总控，记录目标 app、平台范围、默认市场、重点问题、关键词 watchlist、候选池、报告索引、运行约束。
- `focus-apps.md`：当前重点关注 app 清单，是 reporting 的首选输入。
- `reports/`：给人读的 Markdown 报告。
- `snapshots/`：给后续环比和自动化读的 JSON 快照。
- `evidence/`：来源链接、版本证据、截图 URL 清单、素材快照清单。

详细字段与证据规则见：
- [references/file-contracts.md](references/file-contracts.md)
- [references/evidence-rules.md](references/evidence-rules.md)
- [references/mcp-playbook.md](references/mcp-playbook.md)

## Core Rules

- `default_market` 必须在项目文件中显式声明后，discovery 和 reporting 才能进入采集流程。
- `focus-apps.md` 只保存“当前重点关注”的 app；不要把所有候选都塞进去。
- discovery 必须按规则自动给候选打分并生成 `Track` / `Watch` / `Drop` 决策；不要把决策门槛推回给用户确认。
- 需要人工复核的内容，写进 discovery 报告或 reporting 报告的“审核备注 / 不确定项”段落，而不是阻塞流程。
- reporting 默认优先读取 `focus-apps.md`；若为空，再回退到 `app-compete.md` 的 `target_app` 和候选池。
- 任何“关键词显著波动”“素材变化”“版本变化”结论都必须带证据，不能只给主观判断。
- 如果 MCP 能力缺失或返回字段不完整，必须在输出里标注缺口与降级路径。

## Maintenance Workflow

1. 初始化项目时，创建目录与基础模板。
2. 每次变更配置后，更新 `app-compete.md` 的 `last_updated` 与变更说明。
3. 每次深度报告后，把报告路径写入 `report_history`，并把最近一次快照路径记入项目文件。
4. 每次 discovery 完成后，按自动决策更新 `focus-apps.md` 或候选池摘要，并把需要复核的点写进报告。

## References

- [references/workflow-map.md](references/workflow-map.md) - 总控路由与交付边界
- [references/file-contracts.md](references/file-contracts.md) - 项目文件职责与字段约定
- [references/evidence-rules.md](references/evidence-rules.md) - 证据、时间戳与素材判断规则
- [references/mcp-playbook.md](references/mcp-playbook.md) - 两套 MCP 的分工与优先级
- [examples/init-project.md](examples/init-project.md) - 初始化项目示例
- [examples/discovery-review.md](examples/discovery-review.md) - 新增 app 审核示例
- [examples/reporting-run.md](examples/reporting-run.md) - 深度报告示例
