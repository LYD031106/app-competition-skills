# App Competition Workflow Map

## Routing Order

1. 先看 `docs/app-competition/` 是否存在。
2. 再看 `app-compete.md` 是否声明了：
   - `target_app`
   - `platforms`
   - `default_market`
3. 根据用户意图路由：
   - 初始化目录、改模板、更新重点问题：`$app-competition-core`
   - 找新增 app、筛候选、决定是否纳入：`$app-competition-discovery`
   - 写深度报告、做环比、产出 Markdown + JSON：`$app-competition-reporting`

## Output Boundaries

- discovery 的终点是“发现报告 + 自动决策 + 长期文件更新”，不是最终深度报告。
- reporting 的终点是“Markdown 报告 + JSON 快照 + 项目索引更新”。
- core 负责目录、模板、字段与路由，不抢 discovery 或 reporting 的主体分析工作。

## Decision Gates

- discovery 不应要求用户逐条确认候选；应按规则自动给出 `Track` / `Watch` / `Drop`，并据此写入长期文件。
- 需要人工复核的点，写在 discovery 或 reporting 报告的审核备注里。
- 没有 `default_market` 时，任何分析都必须暂停。
- 没有历史快照时，reporting 只能输出 baseline，不得伪造变化趋势。
