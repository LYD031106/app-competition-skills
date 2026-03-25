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

## Material Change Rule

- 截图 URL 变化：可记为“发现明确素材变化”。
- 截图 URL 未变化：只能记为“未发现明确素材变化”，不能断言视觉完全未变。
- 只有口头猜测，没有证据：不要写进结论。

## Keyword Change Rule

- 只有当前一次采集：写 baseline，不写升降趋势。
- 有历史 JSON 快照：比较当前分数、top apps、竞争度与流量解释，再判断变化。
- `get_keyword_scores` 属于启发式分数，必须标注为近似指标，不当成精确排名。

## Release Change Rule

- 优先比较版本号、发布日期、changelog 或 release notes。
- Android 历史常受限，只能稳定拿到最新版本时，要明确写出局限。

## Recommended Evidence Files

- `evidence/discovery-YYYY-MM-DD.md`
- `evidence/material-snapshot-YYYY-MM-DD.md`
- `evidence/release-notes-YYYY-MM-DD.md`

如果没有单独落文件，至少要把证据链接写进报告和 JSON 快照。
