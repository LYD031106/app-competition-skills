# Monthly Comparison Rules

## Comparison Window

- 月报默认比较“上一个完整自然月”的最近一次快照。
- 如果该月没有快照，就回退到最近一次历史快照，并写出精确日期。
- 如果完全没有历史，只能输出 `baseline`。

## Release Rules

- 最近 30 天内有发版，记为 `released_within_30d=true`。
- 每个相关 app 至少记录：
  - `version`
  - `updated_at`
  - `whats_new`
  - `release_theme`
  - `strategic_signal`
- `release_theme` 建议只用以下四类：
  - `new capability`
  - `quality/performance`
  - `pricing/packaging`
  - `seasonal/marketing`

## Screenshot Rules

- 永远先看前三张截图，因为用户最容易先看到这三张。
- 每次快照至少记录：
  - `position`
  - `url`
  - `headline`
  - `semantic_summary`
- 变化判断优先级：
  - URL 变化
  - 顺序变化
  - 主题变化
- 如果发生变化，必须补：
  - `before`
  - `after`
  - `message_shift`
  - `likely_reason`
  - `market_change_hypothesis`
- URL 没变时，只能写“未发现明确更换”。

## Keyword Rank Rules

- 关键词集合来源：
  - `keyword_watchlist`
  - 当期 `analyze_top_keywords` 识别出的高流量词
- 每个关键词至少记录：
  - `keyword`
  - `app_name`
  - `current_rank`
  - `previous_rank`
  - `alert_type`
  - `traffic_score`
- 剧烈变化阈值建议固定为：
  - 进入或跌出 `Top 10`
  - 进入或跌出 `Top 20`
  - `Top 20` 内名次变化 `>= 5`
  - 上期可见、本期消失

## Derived Metrics

建议同时维护：
- `top10_coverage_count`
- `top20_coverage_count`
- `traffic_weighted_visibility`

## Interpretation Guardrails

- 不能因为一个关键词突然波动，就自动断言市场格局变化。
- 优先检查是否和发版、截图更新、标题文案变化、季节性事件相关。
- 如果没有足够证据，只能写“可能相关”，不能写成确定因果。
- 如果多个信号能串起来，优先把它写成 `change_chain`。

## Monthly Completeness Gate

月报完成前，至少自查这 6 项：
- 是否写明 `comparison_window`
- 是否覆盖最近 30 天发版与 `What's New`
- 是否检查前三张截图并记录语义摘要
- 是否输出关键词 rank 快照与 alert
- 是否写出至少一个 `change_chain` 或明确说明为什么没有形成变化链
- 是否显式暴露 `limitations`
