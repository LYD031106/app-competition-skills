# App Competition File Contracts

## docs/app-competition/app-compete.md

项目级总控文件，至少包含：
- `project_name`
- `target_app`
- `platforms`
- `default_market`
- `last_updated`
- `focus_questions`
- `keyword_watchlist`
- `candidate_competitors_pending_review`
- `report_history`
- `latest_snapshot`
- `latest_comparison_window`
- `operational_rules`

适合写这里的内容：
- 项目级约束
- 分析范围
- 重点问题
- 关键词 watchlist
- 候选池 / 观察池摘要
- 报告索引
- 最近一次快照与对比窗口

不适合写这里的内容：
- 所有重点 app 的逐条状态
- 一次性报告正文
- 原始证据全文

## docs/app-competition/focus-apps.md

当前重点关注 app 清单，至少包含：
- `app_name`
- `platform`
- `market`
- `priority`
- `reason_to_watch`
- `watch_type`
- `last_reviewed_at`
- `status`

`watch_type` 推荐值：
- `target`
- `direct`
- `potential`
- `substitute`

`status` 推荐值：
- `active`
- `watching`
- `paused`
- `archived`

自动决策建议：
- `Track` 对应写入 `focus-apps.md`
- `Watch` 对应留在 `app-compete.md` 的候选池或观察池
- `Drop` 只留在当次 discovery 报告

## docs/app-competition/reports/

存放 Markdown 报告。命名建议：
- `YYYY-MM-DD-<slug>-competition-report.md`

报告类型建议：
- `baseline`
- `pulse`
- `monthly-deep-dive`

## docs/app-competition/snapshots/

存放 JSON 快照。命名建议：
- `YYYY-MM-DD-<slug>-competition-report.json`

快照至少应支持：
- `metadata.report_type`
- `metadata.comparison_window`
- `keyword_analysis.rank_tracking`
- `keyword_analysis.alerts`
- `release_analysis.apps`
- `material_analysis.apps`

## docs/app-competition/evidence/

存放来源、链接、截图 URL 清单、素材快照清单与其他证据文件。

推荐子目录：
- `evidence/materials/YYYY-MM-DD/<app>/`：截图 URL、截图说明、前三张语义摘要
- `evidence/releases/`：`What's New`、版本与时间窗口证据
- `evidence/keywords/`：关键词 rank 快照与异常波动清单

## Priority Rule

- `focus-apps.md` 是重点观察对象列表。
- `app-compete.md` 是项目总控与索引。
- 两者职责分离，不要把同一批详细条目重复维护两遍。
