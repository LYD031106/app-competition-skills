---
name: app-competition-reporting
description: Use when generating competitor research reports, monthly deep dives, or overseas-first app market reports that compare recent releases and What's New, top-3 screenshot diffs, and keyword rank changes across reporting windows.
---

# App Competition Reporting

## Overview

负责“竞品报告成稿”这条工作流。目标不是把所有采集结果原样堆进 Markdown，而是把当前轮事实整理成面向产品/ASO 负责人的决策报告。

默认优先回答：
- 主市场这期发生了什么
- 最近发版和 `What's New` 有没有值得盯的动作
- 前三张宣传图与上个窗口相比有没有变化
- 高流量关键词 rank 有没有告警、还有什么机会

## Preconditions

开始前必须：
- 确认 `appstore-mcp` 与 `mcp-appstore` 可用；缺失时先看 [../app-competition-core/references/mcp-installation.md](../app-competition-core/references/mcp-installation.md)
- 读取 `docs/app-competition/app-compete.md`
- 读取 `docs/app-competition/focus-apps.md`

默认先读取：
- [references/report-rendering-style.md](references/report-rendering-style.md)

按模块补读：
- 月报或跨窗口深度对比：读 [references/monthly-deep-dive-playbook.md](references/monthly-deep-dive-playbook.md)
- 发版模块：读 [references/release-watch-contract.md](references/release-watch-contract.md)
- 前三张截图模块：读 [references/top3-screenshot-diff-contract.md](references/top3-screenshot-diff-contract.md)
- 关键词模块：读 [references/keyword-rank-alert-contract.md](references/keyword-rank-alert-contract.md)
- 交付前自查：读 [references/monthly-quality-gate.md](references/monthly-quality-gate.md)

## Market And Scope Rules

市场解析顺序固定为：
1. 用户明确要求的市场
2. `primary_market` / `secondary_markets`
3. 兼容旧项目的 `default_market`
4. 若都没有，使用 `US` 为主、`CN` 为辅，并在输出中写明这是默认初始化逻辑

对象选择顺序固定为：
1. `focus-apps.md` 中 `status=active` 的重点关注 app
2. `app-compete.md` 的 `core_competitors`
3. 观察池中本轮触发 release / material / keyword alert 的 app
4. 如果仍为空，再回退到 `target_app`

## Report Types

- `baseline`：没有历史快照时使用。只写当前格局，不伪造升降趋势。
- `pulse`：短窗口检查。只写本期变化，且必须区分“真实变化”和“新纳入观察对象”。
- `monthly-deep-dive`：月报。必须覆盖主市场快照、近 30 天发版与 `What's New`、前三张宣传图跨窗口变化、高流量关键词 rank 剧烈变化。

优先级：
1. 用户明确要求“月报 / 上个月对比 / monthly / deep dive”时，使用 `monthly-deep-dive`
2. 用户明确要求“本周 / weekly / quick pulse / 临时检查”时，使用 `pulse`
3. 无历史快照时自动回退为 `baseline`

## Workflow

### 1. 读取范围、历史与新鲜度

提取：
- `target_app`
- `platforms`
- `primary_market`
- `secondary_markets`
- `report_focus`
- `keyword_watchlist`
- `core_competitors`
- `watch_pool`
- 上一次报告路径、最近一次 JSON 快照路径、最近一次 discovery evidence 路径

如果历史快照存在，优先做环比；如果不存在，当前报告标记为 `baseline`。

### 2. 采集当前轮快照

主市场必采：
- 关键词：`get_keyword_scores`、`analyze_top_keywords`、`search_app`
- 发版：`get_version_history`、`get_app_details`、`appstore-mcp.get_app_info(include=release)`
- 素材：`appstore-mcp.get_app_info(include=screenshots)`、`get_app_details`

支撑模块只在能解释主线变化时进入正文：
- 评分 / 评论
- 定价 / 订阅
- 开发者与产品线
- 相似 app / 竞品池扩展

### 3. 做变化判断

变化判断优先基于“当前快照 vs 最近一次 JSON 快照”：
- 发版：比较版本、日期、`What's New` 与用户可感知变化
- 素材：只盯前三张，优先看 URL、顺序、主题是否变化
- 关键词：比较实际 rank、阈值告警和是否存在 `change_chain`

没有可靠历史时：
- `baseline` 只写当前基线
- `pulse` 只能写已证实的变化，不把新纳入观察对象伪装成市场剧变

### 4. 生成 Markdown 报告

月报与“决策优先”报告默认使用：
- [assets/monthly-deep-dive-template.md](assets/monthly-deep-dive-template.md)

正文默认结构：
1. `TL;DR`
2. `三问执行摘要`
3. `<Primary Market> Market Snapshot`
4. `Release Watch / What's New`
5. `Top-3 Screenshot Diff vs Last Window`
6. `Keyword Rank Shocks And Opportunities`
7. `What It Means For Us`
8. `CN Cross-Check`（仅在能解释主市场时出现）
9. `Evidence Appendix`
10. `Limitations`

### 5. 生成 JSON 快照

默认使用：
- [assets/monthly-deep-dive-snapshot-template.json](assets/monthly-deep-dive-snapshot-template.json)
- [assets/monthly-deep-dive-snapshot.schema.json](assets/monthly-deep-dive-snapshot.schema.json)

JSON 至少要保留：
- `metadata.market_scope`
- `metadata.latest_collection_at`
- `report_rendering`
- `summary_cards`
- `market_snapshot`
- `release_analysis.apps`
- `material_analysis.apps`
- `material_analysis.changes`
- `keyword_analysis.rank_tracking`
- `limitations`

### 6. 回写项目文件

报告完成后：
- 更新 `app-compete.md` 的 `report_history`
- 更新 `latest_snapshot`
- 更新 `latest_comparison_window`
- 更新 `latest_collection_at`
- 更新 `focus-apps.md` 中涉及 app 的 `last_reviewed_at`

## Output Rules

- 报告必须是“项目报告”，不是“API 字段转抄”
- 正文默认用 `app_name`，不要大量暴露 `app_id`
- `app_id` 只保留在附录、JSON、证据备注和必要的歧义消解中
- `TL;DR` 只写结论、风险、动作，不写 `report_type`、`comparison_window` 这类流程字段
- 正文必须先回答三问：发版、前三张图、关键词告警
- 表格优先窄表化：事实在表内，推断和局限在表后
- 必须区分 `fact`、`inference`、`limitation`
- 关键词模块必须写真实 rank，不能只写启发式流量分
- 没有历史时必须显式降级为 `baseline` 或 `current baseline`

## Common Mistakes

- 直接拿旧快照拼报告，不做当前轮采集
- 在正文里堆太多 `app_id`、字段名或英文工程标签
- 把 `CN` 内容插进主市场叙事中段
- 把“新纳入观察对象”写成“市场发生显著变化”
- 没有证据就断言截图大改或关键词剧烈波动
- 只给 Markdown，不给 JSON 快照

## References

- [references/monthly-deep-dive-playbook.md](references/monthly-deep-dive-playbook.md)
- [references/release-watch-contract.md](references/release-watch-contract.md)
- [references/top3-screenshot-diff-contract.md](references/top3-screenshot-diff-contract.md)
- [references/keyword-rank-alert-contract.md](references/keyword-rank-alert-contract.md)
- [references/report-rendering-style.md](references/report-rendering-style.md)
- [references/monthly-quality-gate.md](references/monthly-quality-gate.md)
- [../app-competition-core/references/evidence-rules.md](../app-competition-core/references/evidence-rules.md)
- [../app-competition-core/references/mcp-playbook.md](../app-competition-core/references/mcp-playbook.md)
- [../app-competition-core/references/mcp-installation.md](../app-competition-core/references/mcp-installation.md)
