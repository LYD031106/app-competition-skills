# App Competition Evidence Rules

## Every Important Claim Needs Evidence

任何关键判断至少要附带以下字段中的大部分：
- `collected_at`
- `market`
- `market_role`（`primary` / `secondary`）
- `platform`
- `app_name`
- `app_id`
- `source_tool`
- `source_link`
- `source_note`

如果是宣传图 / 截图证据，额外至少记录：
- `asset_type`（`store_screenshot` / `promo_card` / `web_capture` / `placeholder_filtered`）
- `position`
- `remote_url`
- `local_path`
- `doc_targets`
- `doc_embed_status`

## Fact / Inference / Limitation

- `fact`：能直接从 MCP 返回结果或历史快照读到。
- `inference`：基于多个事实做出的判断，必须说明推理依据。
- `limitation`：数据缺口、平台限制、历史缺失或截图 URL 无法证明视觉未变。

不要把 `inference` 写成 `fact`，也不要把 `limitation` 藏起来。

## Freshness Rule

- reporting 必须在当前轮执行里刷新关键模块，至少包括：
  - 最近 30 天发版与 `What's New`
  - 当前前三张截图
  - 当前关键词 rank 快照
- 历史 JSON 快照只用于对比，不可代替当前轮采集。
- 如果使用了旧数据或缓存数据，必须写出精确采集日期。

## Comparison Window Rule

- 周报或月报必须写出精确对比窗口，例如 `2026-02-01` 到 `2026-02-29`。
- 月报默认比较“上一个完整自然月”；若缺失，则回退到最近一次历史快照，并写清该快照日期。
- 没有历史快照时，当前报告只能标记 `baseline`。

## Material Change Rule

- 只要前三张截图 URL、顺序或主题发生变化，就应优先记录为“首页关键信息变化”。
- 截图 URL 变化：可记为“发现明确素材变化”。
- 截图 URL 未变化：只能记为“未发现明确素材变化”，不能断言视觉完全未变。
- 前三张截图必须额外记录 `position`、`url`、`headline`、`semantic_summary`。
- 对“为什么换图”的判断属于 `inference`，必须引用更新后的图内容、相关发版信息、关键词信号或市场主题。

截图采集顺序固定为：
1. `appstore-mcp.get_app_info(include=screenshots)`
2. `mcp-appstore.get_app_details`
3. Apple 页面 HTML 回退

宣传图固化顺序固定为：
1. 优先保存 storefront 原图 URL 对应的图片文件
2. 原图 URL 不可下载时，再保存网页回退可见的真实截图证据
3. 只有前两者都失败时，才允许保留纯链接证据并把 `doc_embed_status` 记为 `skipped`

额外素材证据至少要区分：
- `screenshots`
- `hero/share image`
- `placeholder`
- `unverified`

本地文件命名建议固定为：
- `01-top1.png`
- `02-top2.png`
- `03-top3.png`
- `manifest.md`

`doc_embed_status` 推荐值：
- `pending`
- `inserted_summary`
- `inserted_detail`
- `inserted_summary_and_detail`
- `skipped`
- `failed`

如果工具字段为空但网页回退检出真实 storefront 截图：
- 记录为 `tool_empty_web_present`
- 写明 `source_tool=webpage-fallback`
- 不要写成“页面无截图”

只有工具和网页都为空时，才允许写：
- `tool_empty_web_empty`
- `App Store 页面无截图`

## Keyword Change Rule

- 只有当前一次采集：写 baseline，不写升降趋势。
- 有历史 JSON 快照：比较当前实际 rank 与上期 rank，再判断变化。
- `keyword suggestions`、`volume`、`difficulty`、近似评分只能作为附录级解释，不当成主判断依据。
- 关键词告警阈值建议固定为：
  - 进入或跌出 `Top 10`
  - 进入或跌出 `Top 20`
  - `Top 20` 内名次变化 `>= 5`
  - 上期可见、本期消失
- 关键词 evidence 至少要有：
  - `keyword`
  - `app_name`
  - `app_store_url`
  - `target_rank`
  - `previous_rank`
  - `leader_app_name`
  - `leader_app_store_url`
  - `leader_rank`
  - `alert_type`
  - `collected_at`

## Release Change Rule

- 优先比较版本号、发布日期、changelog 或 release notes。
- 最近 30 天内发版的 app，至少记录：`released_within_30d`、`updated_at`、`version`、`whats_new_raw`、`whats_new_summary`
- `What's New` 额外归类为：
  - `new capability`
  - `quality/performance`
  - `pricing/packaging`
  - `seasonal/marketing`
- Android 历史常受限，只能稳定拿到最新版本时，要明确写出局限。

## Cross-Market Rule

- 默认先看 `primary_market`，再决定 `secondary_markets` 是否需要进入正文。
- 次市场只在以下场景应进入正文：
  - 为主市场结论提供对照信号
  - 提供提前出现的产品或素材趋势
  - 暴露主市场尚未出现的新机会或新威胁
- 不要让次市场素材覆盖或稀释主市场叙事。

## Change Chain Rule

- 如果你怀疑某个市场变化成立，优先检查是否同时存在以下至少两类信号：
  - 发版与 `What's New`
  - 前三张截图或素材表达变化
- 目标产品关键词 rank 变化
- 只有单个信号时，可以写“可能相关”，不要写成确定结论。
- 如果三类信号能串起来，优先在报告里明确写出“发版 -> 素材 -> 关键词”的变化链。

## Delivery Evidence Rule

如果开启飞书交付，当前轮至少要保留：
- `summary_doc.doc_id`
- `summary_doc.doc_url`
- `detail_doc.doc_id`
- `detail_doc.doc_url`

主摘要中的每条结论都应能回链到详细子文档或证据文件，而不是只停留在口头结论。

## Recommended Evidence Files

- `evidence/discovery/discovery-YYYY-MM-DD.md`
- `evidence/materials/YYYY-MM-DD/<app>/manifest.md`
- `evidence/materials/YYYY-MM-DD/<app>/01-top1.png`
- `evidence/materials/YYYY-MM-DD/<app>/02-top2.png`
- `evidence/materials/YYYY-MM-DD/<app>/03-top3.png`
- `evidence/releases/release-notes-YYYY-MM-DD.md`
- `evidence/keywords/keyword-ranks-YYYY-MM-DD.md`

如果没有单独落文件，至少要把证据链接写进报告和 JSON 快照。



