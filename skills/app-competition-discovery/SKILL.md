---
name: app-competition-discovery
description: Use when finding newly relevant competitor apps in a configured market, expanding a watchlist, checking charts or similar apps, or deciding whether an app should be added to focus-apps.md or the candidate pool.
---

# App Competition Discovery

## Overview

负责“发现新增 app 竞品”这条工作流。目标是自动完成“发现、筛选、评分、决策、落库”，并把不确定项写进报告，而不是要求用户逐条确认。

## Preconditions

开始前必须先读取：
- `docs/app-competition/app-compete.md`
- `docs/app-competition/focus-apps.md`

如果以下任一条件不满足，先停下来补配置：
- `appstore-mcp` 或 `mcp-appstore` 不可用；此时先引用 `../app-competition-core/references/mcp-installation.md`
- `default_market` 未声明
- `platforms` 未声明
- `target_app` 为空，且 `focus-apps.md` 中也没有任何目标或直接竞品

项目文件职责见：
- [../app-competition-core/references/file-contracts.md](../app-competition-core/references/file-contracts.md)

## Workflow

### 1. 读取当前范围

从 `app-compete.md` 中提取：
- `target_app`
- `platforms`
- `default_market`
- `focus_questions`
- `keyword_watchlist`
- 当前候选池说明

从 `focus-apps.md` 中提取：
- 当前重点关注 app
- `watch_type`
- `priority`
- 最近一次复查时间

市场规则：
- 所有搜索、榜单、相似应用采集都优先使用 `default_market`。
- `US` 是推荐默认市场，但不应被擅自假设。
- 只有在项目文件显式声明双平台时，才做双平台 discovery。

### 2. 生成发现种子

优先使用以下种子组合：
- 目标 app 名称
- `focus-apps.md` 中 `watch_type=target` 或 `watch_type=direct` 的 app
- `keyword_watchlist` 中的关键词

如果用户只给了一个 app 名称，也允许用单 app 作为种子启动发现。

### 3. 拉取候选 app

按以下顺序采集：

1. `mcp-appstore`
   - `get_similar_apps`：从目标 app 或直接竞品扩散
   - `search_app`：按 app 名与关键词搜索双平台候选
   - `get_developer_info`：查看开发者是否有邻近产品线
2. `appstore-mcp`
   - `search_apps`：快速核验 iOS 搜索结果
   - `get_trending_apps`：按类目、平台、榜单补充 iOS 候选
   - `get_app_info`：补 iOS 基础信息、截图、发布日期

两套 MCP 的分工见：
- [../app-competition-core/references/mcp-playbook.md](../app-competition-core/references/mcp-playbook.md)
- 如果 MCP 不可用或用户明确说“还没安装”，先看 [../app-competition-core/references/mcp-installation.md](../app-competition-core/references/mcp-installation.md)

### 4. 初筛与分类

对候选进行以下判断：
- 是否与目标 app 在功能、用户、关键词或场景上明显重叠
- 是否已经在 `focus-apps.md` 或候选池中
- 是否近期仍活跃：有近更新、仍在榜单、仍有完整商店页
- 更适合归类为 `direct`、`potential` 还是 `substitute`

不满足基本重叠条件的 app 不进入推荐清单。

### 5. 生成候选审核清单

默认输出 Markdown 审核表，至少包含：
- `app_name`
- `platform`
- `market`
- `watch_type_suggestion`
- `why_now`
- `source_summary`
- `evidence_links`
- `risks_or_unknowns`
- `score`
- `decision`
- `next_check_date`
- `suggested_action`

评分建议使用以下信号：
- `业务重合度`
- `增长信号`
- `战略风险`
- `差异化价值`
- `可验证性`

决策建议使用以下门槛：
- `Track`：`score >= 70`
- `Watch`：`50 <= score < 70`
- `Drop`：`score < 50`

### 6. 自动写入与留痕

按自动决策直接执行：
- `Track`：写入 `focus-apps.md`
- `Watch`：写入 `app-compete.md` 的候选池或观察池摘要
- `Drop`：不进入长期跟踪清单，但在本次 discovery 报告中保留结论

同时必须：
- 在 `evidence/` 中补一条本次发现记录，或在报告中补完整证据段
- 把 `risks_or_unknowns` 写进报告的审核备注，而不是等待用户确认

## Output Contract

发现流程的默认产物是“发现报告 / 审核清单”，不是深度分析报告。回答应优先给出：
- 新发现的 app 有哪些
- 为什么值得看
- 哪些证据支持
- 哪些地方还不确定
- 系统自动给出的 `Track` / `Watch` / `Drop` 决策
- 建议下一步操作

不要把 discovery 输出伪装成深度报告。

## Common Mistakes

- 直接把搜索结果全量写进 `focus-apps.md`
- 没装好 MCP 就开始假装做发现，最后给出缺证据的结论
- 没有 `default_market` 就开始抓榜单或关键词
- 默认把 `CN` 当作市场，而不是遵守项目里的 `default_market`
- 只看单一平台，忽略项目声明的双平台范围
- 把“相似 app 返回了”直接等同于“值得长期跟踪”
- 没有评分和理由就直接写入重点关注清单
- 把需要复核的问题变成阻塞流程的确认门

## References

- [../app-competition-core/references/workflow-map.md](../app-competition-core/references/workflow-map.md)
- [../app-competition-core/references/evidence-rules.md](../app-competition-core/references/evidence-rules.md)
- [../app-competition-core/references/mcp-installation.md](../app-competition-core/references/mcp-installation.md)
- [../app-competition-core/assets/focus-apps-template.md](../app-competition-core/assets/focus-apps-template.md)
- [../app-competition-core/examples/discovery-review.md](../app-competition-core/examples/discovery-review.md)
