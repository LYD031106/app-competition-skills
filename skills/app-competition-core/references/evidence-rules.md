# App Competition Evidence Rules

## Every Important Claim Needs Evidence

任何关键判断至少要附带以下字段中的大部分：
- `collected_at`
- `market`
- `platform`
- `app_name`
- `app_id`
- `source_tool`
- `source_link`
- `source_note`

## Fact / Inference / Limitation

- `fact`：能直接从 MCP 返回结果或历史快照读到。
- `inference`：基于多个事实做出的判断，必须说明推理依据。
- `limitation`：数据缺口、平台限制、历史缺失或截图 URL 无法证明视觉未变。

## Comparison Window Rule

- 周报或月报必须写出精确对比窗口，例如 `2026-02-01` 到 `2026-02-29`。
- 如果没有“上一个完整自然月”的快照，就回退到最近一次历史快照，并在报告里写出该快照日期。
- 没有历史快照时，当前报告只能标记 `baseline`。

## Material Change Rule

- 只要前三张截图 URL、顺序或主题发生变化，就应该优先记录为“首页关键信息发生变化”。
- 截图 URL 变化：可记为“发现明确素材变化”。
- 截图 URL 未变化：只能记为“未发现明确素材变化”，不能断言视觉完全未变。
- 前三张截图必须额外记录 `position`、`url`、`headline`、`semantic_summary`。
- 对“为什么换图”的判断属于 `inference`，必须引用更新后的图内容、当前市场主题或相关发版信息。

## Keyword Change Rule

- 只有当前一次采集：写 baseline，不写升降趋势。
- 有历史 JSON 快照：比较当前分数、top apps、竞争度、流量解释与实际 rank，再判断变化。
- `get_keyword_scores` 属于启发式分数，必须标注为近似指标，不当成精确排名。
- 关键词告警阈值建议固定为：
  - 进入或跌出 `Top 10`
  - 进入或跌出 `Top 20`
  - `Top 20` 内名次变化 `>= 5`
  - 上期可见、本期消失
- 关键词 evidence 至少要有：`keyword`、`app_name`、`current_rank`、`previous_rank`、`alert_type`、`collected_at`

## Release Change Rule

- 优先比较版本号、发布日期、changelog 或 release notes。
- 最近 30 天内发版的 app，至少记录：`released_within_30d`、`updated_at`、`version`、`whats_new`
- `What's New` 要尽量保留原文或可靠摘录，并额外归类为：
  - `new capability`
  - `quality/performance`
  - `pricing/packaging`
  - `seasonal/marketing`
- Android 历史常受限，只能稳定拿到最新版本时，要明确写出局限。

## Change Chain Rule

- 如果你怀疑某个市场变化成立，优先检查是否同时存在以下至少两类信号：
  - 发版与 `What's New`
  - 前三张截图或素材表达变化
  - 高流量关键词 rank 变化
- 只有单个信号时，可以写“可能相关”，不要写成确定结论。
- 如果三类信号能串起来，优先在报告里明确写出“发版 -> 素材 -> 关键词”的变化链。

## Recommended Evidence Files

- `evidence/discovery-YYYY-MM-DD.md`
- `evidence/material-snapshot-YYYY-MM-DD.md`
- `evidence/release-notes-YYYY-MM-DD.md`
- `evidence/keywords/keyword-ranks-YYYY-MM-DD.md`

如果没有单独落文件，至少要把证据链接写进报告和 JSON 快照。
