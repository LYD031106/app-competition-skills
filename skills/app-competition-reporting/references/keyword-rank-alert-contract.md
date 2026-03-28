# Keyword Rank Alert Contract

## Required Grouping

关键词模块固定分为：
- `Defensive Keywords`
- `Opportunity Keywords`

必要时可补：
- `Boundary Keywords`

## Required Fields

每条记录至少包含：
- `keyword`
- `target_rank`
- `leader_app_name`
- `leader_app_id`
- `leader_rank`
- `previous_rank`
- `delta`
- `alert_type`

可选辅助字段：
- `traffic_score`
- `difficulty_score`
- `change_chain`

## Thresholds

剧烈变化优先检查：
- 进入或跌出 `Top 10`
- 进入或跌出 `Top 20`
- `Top 20` 内名次变化 `>= 5`
- 上期可见、本期消失

## Rendering Rules

正文主表优先显示：
- `keyword`
- `我方 rank`
- `头部对手`
- `对手 rank`
- `当前判断`
- `建议动作`

`traffic_score` 和 `difficulty_score` 不占主表中心。

## Interpretation Rules

- 没有历史 rank：明确写 `baseline`
- 只有实际 rank 才能进入主表
- 只有当发版、截图或标题变化能解释时，才允许写 `change_chain`
- 不要因为单个关键词波动就自动断言市场格局变化
