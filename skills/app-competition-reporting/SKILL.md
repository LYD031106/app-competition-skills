---
name: app-competition-reporting
description: Use when generating competitor research reports, monthly deep dives, or overseas-first app market reports that compare recent releases, top-3 screenshot diffs, and target keyword rank changes across reporting windows.
---

# App Competition Reporting

## Overview

负责“竞品报告成稿”这条工作流。目标不是把所有采集结果原样堆进 Markdown，而是把当前轮事实整理成面向产品 / ASO 负责人和管理层的决策报告。

如果项目声明了 `delivery_channel=feishu` 或 `dual`，这条工作流默认产出 3 层结果：
- 本地 Markdown 完整详细报告
- 1 份飞书主摘要文档
- 1 份飞书详细子文档

详细子文档复用当前长报告主体结构，只补“决策入口层、飞书友好排版、回链主摘要”，不删除现有 `Release / Screenshot / Keyword / Evidence` 内容。
飞书创建与更新默认通过 [$lark-doc](E:/codex/skills/lark-doc/SKILL.md) 执行；如果目标位是知识库节点，再配合 wiki 相关能力落位。

默认优先回答：
- 目标产品 `target_keywords` 有没有真实位移
- 哪些竞品改了 `Name / Title / 前三张宣传图`
- 这些变化对我方目标产品关键词表现意味着什么
- 现在最该做什么

如果 `target_app`、核心竞品或关键词头部结果里存在“同名 / 近同名但不是同一个 app”的情况，也必须把它当成正文级风险处理，而不是埋在附录里等读者自己发现。

## Preconditions

开始前必须：
- 确认 `appstore-mcp` 与 `mcp-appstore` 可用；缺失时先看 [../app-competition-core/references/mcp-installation.md](../app-competition-core/references/mcp-installation.md)
- 读取 `docs/app-competition/app-compete.md`
- 读取 `docs/app-competition/focus-apps.md`

默认先读取：
- [references/report-rendering-style.md](references/report-rendering-style.md)
- [references/summary-doc-standard.md](references/summary-doc-standard.md)

按模块补读：
- 月报或跨窗口深度对比：读 [references/monthly-deep-dive-playbook.md](references/monthly-deep-dive-playbook.md)
- 发版模块：读 [references/release-watch-contract.md](references/release-watch-contract.md)
- 前三张截图模块：读 [references/top3-screenshot-diff-contract.md](references/top3-screenshot-diff-contract.md)
- 关键词模块：读 [references/keyword-rank-alert-contract.md](references/keyword-rank-alert-contract.md)
- 交付前自查：读 [references/monthly-quality-gate.md](references/monthly-quality-gate.md) 和 [references/summary-review-rubric.md](references/summary-review-rubric.md)
- 如果开启飞书交付：补读 [references/executive-main-doc-contract.md](references/executive-main-doc-contract.md)、[references/child-doc-contract.md](references/child-doc-contract.md)、[references/feishu-delivery-contract.md](references/feishu-delivery-contract.md)

## Market And Scope Rules

市场解析顺序固定为：
1. 用户明确要求的市场
2. `primary_market` / `secondary_markets`
3. 兼容旧项目的 `default_market`
4. 若都没有，使用 `US` 为主、`CN` 为辅，并在输出中写明这是默认初始化逻辑

对象选择顺序固定为：
1. `focus-apps.md` 中 `status=active` 的重点关注 app
2. `app-compete.md` 的 `core_competitors`
3. 观察池中本轮触发 release / material / keyword alert 的 app
4. 如果仍为空，再回退到 `target_app`

## Report Types

- `baseline`：没有历史快照时使用。只写当前格局，不伪造升降趋势。
- `pulse`：短窗口检查。只写本期变化，且必须区分“真实变化”和“新纳入观察对象”。
- `monthly-deep-dive`：月报。必须覆盖主市场快照、近 30 天发版与 `What's New`、前三张宣传图跨窗口变化、目标产品 `target_keywords` 的真实 rank 位移。

优先级：
1. 用户明确要求“月报 / 上个月对比 / monthly / deep dive”时，使用 `monthly-deep-dive`
2. 用户明确要求“本周 / weekly / quick pulse / 临时检查”时，使用 `pulse`
3. 无历史快照时自动回退为 `baseline`

## Workflow

### 1. 读取范围、历史与新鲜度

提取：
- `target_app`
- `platforms`
- `primary_market`
- `secondary_markets`
- `report_focus`
- `target_keywords`
- `core_competitors`
- `watch_pool`
- 上一次报告路径、最近一次 JSON 快照路径、最近一次 discovery evidence 路径

如果历史快照存在，优先做环比；如果不存在，当前报告标记为 `baseline`。

### 2. 采集当前轮快照

主市场必采：
- 关键词：对 `target_keywords` 逐个查询真实 rank，并记录必要旁证与采集时间
- 名称文案：`get_app_details`、`appstore-mcp.get_app_info(include=full)`，用于核对 `Name / Title`
- 商店链接：优先保存 MCP 返回的官方 `trackViewUrl` / `url`；缺失时用 iOS `app_id` 或 Android package name 拼官方商店链接
- 素材：`appstore-mcp.get_app_info(include=screenshots)`、`get_app_details`、必要时 Apple 页面 HTML 回退
- 只有在能解释关键词或素材变化时，才补发版：`get_version_history`、`appstore-mcp.get_app_info(include=release)`

关键词主链默认只走真实 rank，不把 `analyze_top_keywords`、`get_keyword_scores`、`traffic_score`、`difficulty_score` 当成正文主链内容；如项目数据里存在这些字段，只能作为附录或 debug 旁证使用。

支撑模块只在能解释主线变化时进入正文：
- 评分 / 评论
- 定价 / 订阅
- 开发者与产品线
- 相似 app / 竞品池扩展

### 2.5 识别命名歧义

对以下对象做名称近似比对：
- `target_app`
- `focus-apps.md` 中本轮会进入正文的对象
- `core_competitors`
- 主关键词搜索结果里会进入正文或附录重点说明的对象

以下情况默认视为 `name_collision_risk`：
- 只差单复数，如 `Object` / `Objects`
- 只差标点或连接词
- 核心短语完全相同，只多一个轻修饰词
- 同一关键词结果页里出现 2 个以上极近似标题，但 `developer`、定位或产品范围不同

一旦 `target_app` 或正文优先对象命中该风险：
- Markdown 首屏必须在 `一句话结论` 后插入 `Name Collision / Ambiguity Alert`
- 该提示块至少给出 `app_name`、角色、开发者 / 公司短标签、为什么容易混淆
- `app_id` 放到附录、JSON 或证据备注，不放到首屏
- 不能只在附录里补 `app_id` 了事

### 3. 做变化判断

变化判断优先基于“当前快照 vs 最近一次 JSON 快照”：
- 发版：比较版本、日期、`What's New` 与用户可感知变化
- 素材：只盯前三张，优先看 URL、顺序、主题是否变化
- 关键词：比较实际 rank、阈值告警和是否存在 `change_chain`

截图判断必须额外区分：
- `tool_present`
- `tool_empty_web_present`
- `tool_empty_web_empty`
- `unresolved`

`MCP 未返回截图` 只能表示工具采集缺口。
只有网页回退也没有真实截图证据时，才允许写 `App Store 页面无截图`。

没有可靠历史时：
- `baseline` 只写当前基线
- `pulse` 只能写已证实的变化，不把新纳入观察对象伪装成市场剧变

### 4. 生成 Markdown 报告

月报与“决策优先”报告默认使用：
- [assets/monthly-deep-dive-template.md](assets/monthly-deep-dive-template.md)

如果当次要生成飞书主摘要文档，额外使用：
- [assets/executive-main-doc-template.md](assets/executive-main-doc-template.md)

正文默认结构：
1. 判断型标题
2. `决策入口层`
3. `一句话结论`
4. `Name Collision / Ambiguity Alert`（仅在存在同名 / 近同名风险时出现）
5. `目标产品关键词表现`
6. `竞品动态`
7. `建议行动`
8. `Optional Context`
9. `Detail Delivery Context`
10. `Back To Main Summary`
11. `研究证据层`
12. `目标产品关键词详细证据`
13. `竞品 Name / Title / Top-3 Screenshot 证据`
14. `Release Context`（仅在能解释变化时出现）
15. `CN Cross-Check`（仅在能解释主市场时出现）
16. `Evidence Appendix`
17. `Limitations`

飞书主摘要文档默认结构：
1. 判断型标题
2. `封面摘要栏`
3. `一句话判断`
4. `目标产品关键词表现`
5. `竞品变化雷达`
6. `现在要做什么`
7. `子文档入口`
8. `范围与局限`

### 5. 生成 JSON 快照

默认使用：
- [assets/monthly-deep-dive-snapshot-template.json](assets/monthly-deep-dive-snapshot-template.json)
- [assets/monthly-deep-dive-snapshot.schema.json](assets/monthly-deep-dive-snapshot.schema.json)

JSON 至少要保留：
- `metadata.market_scope`
- `metadata.latest_collection_at`
- `app_store_url`（所有进入正文、主摘要、详细证据或附录的 app）
- `delivery_outputs`
- `report_rendering`
- `summary_cards`
- `market_snapshot`
- `release_analysis.apps`
- `material_analysis.apps`
- `material_analysis.changes`
- `keyword_analysis.rank_tracking`
- `limitations`

关键词快照必须围绕 target-only model：
- `keyword_analysis.rank_tracking` 主字段以 `keyword`、`target_rank`、`previous_rank`、`delta`、`leader_app_name`、`leader_rank`、`collected_at` 为中心
- `target_keyword_source` 与 `target_keyword_last_verified_at` 必须保留，便于追溯每个 target keyword 的来源和最后核验时间
- `leader_app_id`、`traffic_score`、`difficulty_score`、`alert_type`、`change_chain` 只能作为辅助字段，不得在主表中抢占中心
- 关键词分组默认写成 `At-Risk Target Keywords` / `Recoverable Target Keywords`，不再围绕广义机会词池组织正文

素材相关 JSON 至少额外保留：
- `screenshot_collection_status`
- `screenshot_sources`
- `device_coverage`
- `webpage_screenshot_evidence`
- `web_fallback_used`

### 6. 回写项目文件

报告完成后：
- 更新 `app-compete.md` 的 `report_history`
- 更新 `latest_snapshot`
- 更新 `latest_comparison_window`
- 更新 `latest_collection_at`
- 如果开启飞书交付，更新 `latest_delivery_summary_doc` 与 `latest_delivery_detail_doc`
- 更新 `focus-apps.md` 中涉及 app 的 `last_reviewed_at`

## Output Rules

- 报告必须是“项目报告”，不是“API 字段转抄”
- 默认读者是产品 / ASO 负责人和管理层，写法必须满足“30 秒看到结论、90 秒抓住重点、3-5 分钟完成决策性扫读”
- 正文默认用可点击的 `app_name`，格式为 `[app_name](app_store_url)`，不要大量暴露 `app_id`
- `目标 App`、`对象`、`竞品名称`、`leader_app_name` 等 app 名称列只要进入报告，都优先写成可点击名称
- `app_store_url` 优先使用 MCP 返回的官方商店链接；缺失时再用 `app_id` / package name 拼接官方链接；仍无法确认时在附录或局限中标记链接缺失
- `app_id` 只保留在附录、JSON、证据备注和必要的歧义消解补充中
- 首屏禁止出现 `app_id`、原始状态码、工具内部标签、精确采集时间戳
- `一句话结论` 只写本期最值得记住的变化，不写 `report_type`、`comparison_window` 这类流程字段
- `target_app` 或正文重点对象如果存在同名 / 近同名风险，必须在目标产品关键词表现和竞品动态之前做显式歧义消解
- 正文必须先回答 4 件事：一句话结论、目标产品关键词表现、竞品实质变化、当前动作
- 主摘要只保留 1 句判断、目标产品关键词表现表、竞品变化表、每个摘要竞品 1 张基于当前 `top1-3` 生成的三联合并图和 1-3 条动作
- 如果开启飞书交付，主摘要对每个进入摘要的竞品都要插入 1 张基于当前 App Store `top1-3` 生成的三联合并图
- 飞书摘要里的竞品图卡默认使用三联合并图；详细子文档继续保留 `top1-3` 逐张原图
- 这些图组放在 `竞品变化雷达` 段内，紧跟窄表之后
- 表格优先窄表化：事实在表内，推断和局限在表后
- `fact`、`inference`、`limitation` 在详细层可以保留；摘要层必须改写成自然业务语言，不直接暴露技术标签
- 同一条核心动作只能有一个主表达位，其他位置只允许补上下文，不重复原句
- `Release Context` 正文默认异常优先；低变化且意义弱的 maintenance 长表下沉到详细层或附录
- 关键词模块默认只写目标产品真实 rank，不能只写启发式流量分
- 默认不展开竞品关键词位移模块，除非用户明确要求
- 没有历史时必须显式降级为 `baseline` 或 `current baseline`
- 如果开启飞书交付，主摘要文档不能塞完整 `Evidence Appendix`
- 详细子文档不能因为“摘要化”而删掉现有详细内容
- `MCP 未返回截图` 和 `App Store 页面无截图` 禁止混写
