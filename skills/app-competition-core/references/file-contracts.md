# App Competition File Contracts

## docs/app-competition/app-compete.md

项目级总控文件，至少包含：
- `project_name`
- `target_app`
- `platforms`
- `primary_market`
- `secondary_markets`
- `default_market`（兼容字段，建议等于 `primary_market`）
- `market_priority_rule`
- `report_language`
- `delivery_channel`
- `feishu_destination`（默认 `app-competition`，表示竞品分析默认父目录 / 节点）
- `summary_doc_mode`
- `detail_doc_mode`
- `creative_asset_delivery_mode`
- `preserve_existing_content`
- `report_goal`
- `comparison_mode`
- `report_focus`
- `focus_questions`
- `target_keywords`
- `target_keyword_source`
- `target_keyword_source_ref`
- `target_keyword_last_verified_at`
- `core_competitors`
- `candidate_competitors_pending_review`
- `watch_pool`
- `report_history`
- `latest_snapshot`
- `latest_comparison_window`
- `latest_discovery_evidence`
- `latest_delivery_summary_doc`
- `latest_delivery_detail_doc`
- `latest_collection_at`
- `operational_rules`

适合写这里的内容：
- 项目级约束
- 市场范围与优先级
- 当前交付方式与飞书目标位
- 宣传图交付模式与是否需要插图
- 重点问题
- 目标产品关键词 contract 与来源
- 核心竞品与观察池摘要
- 报告索引
- 最近一次快照、发现证据、对比窗口与飞书交付入口

不适合写这里的内容：
- 所有重点 app 的逐条状态
- 一次性报告正文
- 原始证据全文

## docs/app-competition/focus-apps.md

当前重点关注 app 清单，至少包含：
- `app_name`
- `app_store_url`
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

链接规则：
- `app_store_url` 优先保存 MCP 返回的官方商店链接，例如 Apple `trackViewUrl` 或 Google Play `url`。
- 如果工具没有直接返回链接，iOS 可用 `https://apps.apple.com/app/id<app_id>` 兜底，Android 可用 `https://play.google.com/store/apps/details?id=<package_name>` 兜底。
- 只有在无法确认 `app_id` / package name 或同名歧义尚未消解时，才允许暂时留空，并在报告 `Limitations` 或证据备注中说明。

## docs/app-competition/reports/

存放 Markdown 报告。命名建议：
- `YYYY-MM-DD-<slug>-competition-report.md`

报告类型建议：
- `baseline`
- `pulse`
- `monthly-deep-dive`

默认正文应优先回答：
- 主市场本期格局
- 最近 30 天发版与 `What's New`
- 前三张宣传图变化
- 目标产品关键词 rank 变化与对应风险 / 恢复空间

当项目开启 `delivery_channel=feishu` 或 `dual` 时：
- 本地 Markdown 报告继续保留为“完整详细版”
- 每次额外新建 1 份飞书主摘要文档
- 每次额外新建 1 份飞书详细子文档
- 主摘要文档不替代 Markdown 正文
- 若 `creative_asset_delivery_mode` 不是 `evidence-only`，还要把代表性宣传图插入飞书文档

## docs/app-competition/snapshots/

存放 JSON 快照。命名建议：
- `YYYY-MM-DD-<slug>-competition-report.json`

快照至少应支持：
- `metadata.market_scope`
- `metadata.latest_collection_at`
- `collection_log.discovery_refreshed`
- `delivery_outputs`
- `market_snapshot.comparison_matrix`
- `scope.target_keywords`
- `scope.target_keyword_source`
- `scope.target_keyword_last_verified_at`
- `keyword_analysis.rank_tracking`
- `keyword_analysis.alerts`
- `keyword_analysis.at_risk_target_keywords`
- `keyword_analysis.recoverable_target_keywords`
- `release_analysis.apps`
- `material_analysis.apps`
- `material_analysis.changes`
- 所有进入正文、主表或附录的 app 的 `app_store_url`

素材相关快照至少额外支持：
- `screenshot_collection_status`
- `screenshot_sources`
- `device_coverage`
- `webpage_screenshot_evidence`
- `web_fallback_used`
- `asset_files`
- `doc_media_status`

## docs/app-competition/evidence/

存放来源、链接、截图 URL 清单、下载后的宣传图文件、素材快照清单与其他证据文件。

推荐子目录：
- `evidence/materials/YYYY-MM-DD/<app>/`：截图 URL、截图说明、前三张语义摘要、已下载图片与 manifest
- `evidence/releases/`：`What's New`、版本与时间窗口证据
- `evidence/keywords/`：关键词 rank 快照与异常波动清单
- `evidence/discovery/`：最新候选审核清单、发现记录

如果发生“工具空截图但网页回退有截图”的情况，证据目录至少要能落：
- `webpage_screenshot_evidence`
- `source_type=webpage-fallback`
- `placeholder_filtered=true`
- `doc_embed_status`

## Priority Rule

- `focus-apps.md` 是重点观察对象列表。
- `app-compete.md` 是项目总控与索引。
- `reports/` 是项目型输出，不是原始数据堆栈。
- `snapshots/` 是结构化历史，用于环比与自动化消费。
- 飞书主摘要文档是管理摘要，不替代 Markdown 正文。
- 飞书详细子文档保留现有详细内容，只是在飞书里新增一层入口。
- 两者职责分离，不要把同一批详细条目重复维护两遍。







