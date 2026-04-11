# Monthly Comparison Rules

## Comparison Window

- 月报默认比较“上一个完整自然月”的最近一次快照。
- 如果该月没有快照，就回退到最近一次历史快照，并写出精确日期。
- 如果完全没有历史，只能输出 `baseline`。

## Market Scope Rules

- 月报默认先写 `primary_market`，再决定 `secondary_markets` 是否需要进入正文。
- 用户没有明确指定时，推荐逻辑是 `US` 为主、`CN` 为辅。
- 次市场只在能解释主市场结论、形成趋势对照或提供新机会时进入正文。

## Fresh Collection Rules

- 月报开始后，必须在当前轮执行里刷新：
  - 目标产品 `target_keywords` 的 rank 快照
  - 竞品 `Name / Title`
  - top-3 screenshots
  - 必要时才补 release / `What's New`
- 旧快照用于对比，不可直接代替当前轮采集。
- 如果任何关键模块无法刷新，必须在报告和 JSON 中暴露局限。

## ASO Interpretation Rules

- 月报默认先确认目标产品关键词真值与 rank 变化，再决定是否调用更深的 ASO 解释层。
- 需要解释变化时，优先用以下顺序：
  - `Name / Title`
  - 前三张截图
  - release / `What's New`
  - ratings / reviews
  - pricing / packaging
- 自动扩词、suggestions pool、volume score 不再作为默认月报主链。

## Name / Title Rules

- 每个进入正文的竞品，至少记录：
  - `app_name`
  - `current_name`
  - `previous_name`
  - `current_title`
  - `previous_title`
  - `name_changed`
  - `title_changed`
- 如果平台没有独立 `Title` 字段：
  - 优先使用 `Subtitle`
  - 仍然没有时，使用 storefront 首屏主标题文案代理
  - 报告里注明口径，不要假装字段原生存在

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
  - `before_theme`
  - `after_theme`
  - `message_shift`
  - `likely_reason`
  - `market_change_hypothesis`
- URL 没变时，只能写“未发现明确更换”。

## Keyword Rank Rules

- 关键词集合只允许来自 `target_keywords`。
- 月报关键词模块只追踪目标产品，不追踪竞品关键词波动。
- 每个关键词至少记录：
  - `market`
  - `keyword`
  - `target_app_name`
  - `target_app_store_url`
  - `target_rank`
  - `previous_rank`
  - `delta`
  - `leader_app_name`
  - `leader_app_store_url`
  - `leader_rank`
  - `collected_at`
  - `alert_type`
  - `note`
- `note` 只写变化背景，例如：
  - 哪个新名字进到前 10
  - 是否存在同名 / 近同名干扰
  - 是否与标题或前三图变化同时发生
- 剧烈变化阈值建议固定为：
  - 进入或跌出 `Top 10`
  - 进入或跌出 `Top 20`
  - `Top 20` 内名次变化 `>= 5`
  - 上期可见、本期消失

## Release Rules

- release 默认不是首屏必写项。
- 最近 30 天内有发版，且能解释 `Name / Title / 前三图 / 关键词位移` 时，才提升到正文。
- 如果进入正文，每个相关 app 至少记录：
  - `previous_version`
  - `current_version`
  - `updated_at`
  - `whats_new_summary`
  - `release_theme`
- `release_theme` 建议只用以下四类：
  - `new capability`
  - `quality/performance`
  - `pricing/packaging`
  - `seasonal/marketing`

## Interpretation Guardrails

- 不能因为一个关键词突然波动，就自动断言市场格局变化。
- 优先检查是否和 `Name / Title` 改动、前三图变化、发版或季节性事件相关。
- 如果没有足够证据，只能写“可能相关”，不能写成确定因果。
- 如果多个信号能串起来，优先把它写成“变化链”。

## Monthly Completeness Gate

月报完成前，至少自查这 7 项：
- 是否写明 `comparison_window`
- 是否记录 `latest_collection_at`
- 是否有 1 句能直接说清重点的结论
- 是否检查前三张截图并记录语义摘要
- 是否输出目标产品关键词 rank 快照与 alert
- 是否只列实质变化的竞品 `Name / Title / 前三图`
- 是否显式暴露 `limitations`
