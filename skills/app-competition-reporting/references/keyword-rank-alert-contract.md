# Keyword Rank Alert Contract

## Goal

关键词模块只服务于目标产品 `target_keywords`，默认主链是逐个查询真实 rank，不再把竞品关键词池或扩词叙事放进正文主链。

## Required Fields

每条记录至少包含：
- `keyword`
- `target_rank`
- `previous_rank`
- `delta`
- `leader_app_name`
- `leader_rank`
- `leader_app_store_url`
- `collected_at`

可选辅助字段：
- `target_keyword_source`
- `target_keyword_last_verified_at`
- `leader_app_id`
- `traffic_score`
- `difficulty_score`
- `change_chain`
- `alert_type`
- `note`

## Grouping

关键词分组默认固定为：
- `At-Risk Target Keywords`
- `Recoverable Target Keywords`

必要时才补充 debug / appendix 分组，不把它们抬到主表中心。

## Thresholds

剧烈变化优先检查：
- 进入或跌出 `Top 10`
- 进入或跌出 `Top 20`
- `Top 20` 内名次变化 `>= 5`
- 上期可见、本期消失

## Rendering Rules

正文主表优先显示：
- `keyword`
- `上期排名`
- `本期排名`
- `变化`
- `头部对手`
- `头部对手排名`
- `collected_at`

`头部对手` 使用 `[leader_app_name](leader_app_store_url)`；如果链接缺失，附录或局限中说明待核验。

`traffic_score`、`difficulty_score`、`leader_app_id`、`target_keyword_source`、`target_keyword_last_verified_at` 只能作为辅助字段，不占主表中心。
默认只列有变化的词，最多 5 个。

## Interpretation Rules

- 没有历史 rank：明确写 `baseline`
- 只有实际 rank 才能进入主表
- 只有当 `Name / Title / 前三图` 或发版能够解释时，才允许写 `change_chain`
- 不要因为单个关键词波动就自动断言市场格局变化
- 默认不展开竞品关键词位移结论，除非用户明确要求
