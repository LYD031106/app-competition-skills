---
name: app-competition-reporting
description: Use when generating weekly or ad hoc app competitor reports, comparing keyword, release, material, pricing, review, or similar-app changes, or writing markdown and JSON competition snapshots.
---

# App Competition Reporting

## Overview

负责“深度竞品分析报告”这条工作流。输出必须同时覆盖人读版 Markdown 与机器读版 JSON，并优先围绕 `focus-apps.md` 中的重点关注对象展开。

## Preconditions

开始前必须：
- 读取 `docs/app-competition/app-compete.md`
- 读取 `docs/app-competition/focus-apps.md`
- 确认 `default_market` 已声明

对象选择顺序固定为：
1. `focus-apps.md` 中 `status=active` 的重点关注 app
2. 如果为空，回退到 `app-compete.md` 的 `target_app`
3. 仍为空时，再回退到候选池摘要并明确说明这是降级路径

## Workflow

### 1. 读取范围与历史

提取：
- `default_market`
- `platforms`
- `focus_questions`
- `keyword_watchlist`
- 上一次报告路径与最近一次 JSON 快照路径

如果历史快照存在，优先做环比；如果不存在，当前报告标记为 `baseline`。

### 2. 采集当前快照

按 app 和平台采集以下模块：

1. 关键词
   - `get_keyword_scores`
   - `analyze_top_keywords`
   - `search_app`
2. 发版与更新
   - `get_version_history`
   - `get_app_details`
   - `appstore-mcp.get_app_info(include=release)`
3. 宣传素材
   - `appstore-mcp.get_app_info(include=screenshots)` 负责 iOS 截图
   - `get_app_details` 负责 Android 截图与头图
4. 评分与评论
   - `get_app_details`
   - `analyze_reviews`
   - 必要时 `fetch_reviews`
5. 定价与订阅
   - `get_pricing_details`
6. 开发者与产品线
   - `get_developer_info`
7. 竞品池变化
   - `get_similar_apps`

### 3. 做变化判断

变化判断优先基于“当前快照 vs 最近一次 JSON 快照”：
- 关键词：比较当前分数、当前 top apps、竞争度和流量解释；没有历史时只给 baseline。
- 发版：比较版本号、发布日期、更新说明。
- 素材：比较截图 URL 列表；URL 变化可视为确定变化，URL 未变时只能说“未发现明确变化”。
- 评分/评论：比较评分、评论量、主要主题。
- 定价：比较订阅、IAP、广告支持与 monetization model。
- 相似 app：比较相似 app 名单与排序。

MCP 不提供可靠历史时，必须明确标注局限，不要伪造“排名变化”。

### 4. 生成 Markdown 报告

默认路径：
- `docs/app-competition/reports/YYYY-MM-DD-<slug>-competition-report.md`

报告结构至少包含：
- 摘要
- 本期关键变化
- 关键词排名与提升空间
- 最近发版与功能变化
- 宣传素材变化
- 评分与评论信号
- 定价与订阅变化
- 开发者/产品线/相似竞品变化
- 结论、风险、下一步建议

可直接参考：
- [../app-competition-core/assets/competition-report-template.md](../app-competition-core/assets/competition-report-template.md)

### 5. 生成 JSON 快照

默认路径：
- `docs/app-competition/snapshots/YYYY-MM-DD-<slug>-competition-report.json`

JSON 至少包含：
- `metadata`
- `scope`
- `apps`
- `keyword_analysis`
- `release_analysis`
- `material_analysis`
- `review_analysis`
- `pricing_analysis`
- `developer_analysis`
- `similar_apps_analysis`
- `highlights`
- `limitations`

模板见：
- [../app-competition-core/assets/snapshot-template.json](../app-competition-core/assets/snapshot-template.json)

### 6. 回写项目文件

报告完成后：
- 更新 `app-compete.md` 的 `report_history`
- 更新最近一次快照路径
- 更新 `focus-apps.md` 中涉及 app 的 `last_reviewed_at`

## Output Rules

- 必须区分 `fact`、`inference`、`limitation`。
- 对“显著波动”“有提升空间”这类判断，必须解释依据。
- 如果只有单次快照，没有历史，就说清楚是“当前基线观察”。
- 回答中应优先给出最重要变化，不要把报告写成纯原始数据堆栈。

## Common Mistakes

- 没有历史快照却硬写“排名上升/下降”
- 没有证据就断言素材大改版
- 只给 Markdown，不写 JSON 快照
- 忽略 `focus-apps.md`，直接把所有候选都拉进报告
- 不更新 `last_reviewed_at` 和 `report_history`

## References

- [../app-competition-core/references/evidence-rules.md](../app-competition-core/references/evidence-rules.md)
- [../app-competition-core/references/mcp-playbook.md](../app-competition-core/references/mcp-playbook.md)
- [../app-competition-core/assets/competition-report-template.md](../app-competition-core/assets/competition-report-template.md)
- [../app-competition-core/assets/snapshot-template.json](../app-competition-core/assets/snapshot-template.json)
- [../app-competition-core/examples/reporting-run.md](../app-competition-core/examples/reporting-run.md)
