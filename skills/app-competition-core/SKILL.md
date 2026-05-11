---
name: app-competition-core
description: Use when initializing or maintaining app competition research workspaces, defining market/delivery/keyword contracts, handling one or many owned apps in a portfolio competitor-analysis run, or coordinating end-to-end competitor research through discovery, reporting, Feishu delivery, email handoff, external access checks, and project index backfill.
---

# App Competition Core

## Overview

这个 skill 是 `docs/app-competition/` 的轻量总控：负责项目 contract、路由、交付门禁和多产品摘要收口。它不直接替代 `$app-competition-discovery` 或 `$app-competition-reporting` 的采集 / 成稿细节。

默认职责：
- 初始化或维护 `docs/app-competition/` 项目结构。
- 解析市场、关键词来源、交付方式和目标产品 contract。
- 单产品报告路由到 discovery / reporting。
- 多自有产品进入 Portfolio Mode，由主 agent 汇总各产品 work unit。
- Feishu / dual 交付默认只创建 1 份飞书摘要文档；详细子文档只有用户明确要求时才创建。
- 回写 `app-compete.md`、`focus-apps.md`、snapshot、delivery manifest 和最新远端链接。

默认市场：用户指定优先，其次项目 `primary_market` / `secondary_markets`，旧 `default_market` 视为 `primary_market`，都缺失时使用 `US` 为主、`CN` 为辅。

默认表达：朴素中文，只写字段、证据、时间、来源；用户未要求建议时，不写动作建议、影响评估或咨询腔总结。

## Quick Start

1. 检查 `docs/app-competition/`、`app-compete.md`、`focus-apps.md` 是否存在；缺失时用 assets 模板初始化。
2. 读取项目 contract：`target_app` / `target_apps`、`primary_market`、`secondary_markets`、`delivery_channel`、`target_keywords`、`target_keyword_source`、`core_competitors`。
3. 检查依赖：`appstore-mcp`、`mcp-appstore`、可选 `appeeky-mcp`、飞书交付所需 `lark-cli` / lark skills。缺失时先运行 `scripts/bootstrap_app_competition.ps1` 或读取 [mcp-installation.md](references/mcp-installation.md)。
4. 关键词来源只允许三层：用户显式提供、`appeeky-mcp.get_app_keywords`、`mcp-appstore.get_app_extracted_keywords`。任何 fallback 必须写 provenance，不能把启发式分数当真实 rank。
5. 单产品报告：先确保 discovery fresh，再交给 `$app-competition-reporting` 生成报告、snapshot 和 evidence。
6. 多产品报告：进入 [Portfolio Mode](#portfolio-mode)，每个自有产品独立 work unit，主 agent 统一生成 1 份组合摘要。
7. Feishu / dual：执行 [Delivery Gate](#delivery-gate)。默认只发布 1 份飞书摘要文档；详细子文档必须由用户明确提出。
8. 邮件或外链：先完成飞书摘要验收，再执行 SMTP 邮件或 external access gate。

## Portfolio Mode

当用户一次给出多个“我的产品 / owned apps / target apps”，不能合并成一个巨大目标产品，也不能只输出多份分散报告。必须为每个自有产品建立独立 work unit，再汇总成一份多产品摘要。

Portfolio work unit 必须包含：
- `product_slug`
- `target_app`
- `platform`
- `primary_market` / `secondary_markets`
- `target_keywords`
- `target_keyword_source`
- `core_competitors`
- `report` / `snapshot` / `evidence` 路径

多产品摘要固定使用 [multi-product-summary-template.md](assets/multi-product-summary-template.md)，并通过 [portfolio-summary-format.md](references/portfolio-summary-format.md) 门禁。结构必须是：

1. `产品关键词记录`
2. 每个自有产品一个一级标题：`# [自有产品] 关键词与竞品字段核查`
3. 每个产品段只包含：
   - `目标产品关键词记录`
   - `目标产品字段核查`
   - `[自有产品] 竞品变化核查`
   - `[自有产品] 竞品宣传图核查`

多产品摘要不要加入额外的封面信息表、事实综述段、产品小摘要表、跨产品竞品索引、归档证据表、局限章节、规则清单或默认二级文档入口。

字段口径：
- 目标产品字段只核查标题 / 名称、产品描述、更新日志。
- 目标产品描述与上一版相同写 `无变化`；有变化才写 `之前的产品描述：...`、`本次产品描述：...`、`改动点：...`。
- 更新日志直接粘贴抓取到的 releaseNotes 原文；没有则写 `没有`。
- 默认不展示自有产品自己的 iPhone top1-3 宣传图。
- 竞品表必须有 `icon` 列，展示抓取到的竞品产品 icon。
- 竞品字段核查必须覆盖标题 / 名称、竞品产品描述、竞品 iPhone 前三图、更新日志。
- 竞品宣传图只展示竞品图：每个竞品单独一个 `### [竞品] 宣传图` 小节，包含前三张 iPhone 宣传图变化、上一版竞品宣传图、本次竞品宣传图。

素材口径：
- 只允许 iPhone App Store 产品页截图作为宣传图证据。
- `appstore-mcp.get_app_info(include=screenshots)`、`mcp-appstore.get_app_details.screenshots`、Apple lookup `screenshotUrls` 非空时，直接作为 iPhone top1-3 来源。
- 官方字段为空且对象进入摘要时，调用 `scripts/fetch_appstore_screenshots.py --device iphone` 做网页回退。
- 图标、Today / Features 推荐图、1x1 占位图、搜索结果卡片图不能冒充产品页前三图。
- 用户可见摘要遇到缺图只写 `无宣传图`；采集管线状态写入 snapshot / manifest / evidence。

## Delivery Gate

完成条件按 `delivery_channel` 判断：

| channel | 完成条件 |
| --- | --- |
| `markdown` | 本地 Markdown、JSON snapshot、evidence、`app-compete.md` 最新索引已回写 |
| `feishu` | 1 份飞书摘要文档已创建 / 更新、链接与图片验收通过、manifest 与索引已回写 |
| `dual` | 同时满足 `markdown` 与 `feishu` |

Feishu / dual 规则：
- 默认只创建 1 份飞书摘要文档。
- 只有用户明确要求“详细子文档 / 第二份文档 / 逐张原图文档”时，才创建详细子文档和 `*-feishu-detail.md` draft。
- 最终回复默认只需要飞书摘要链接；只有用户要求详细子文档时，才必须返回详细子文档链接。
- 摘要中出现的自有产品和竞品名称必须使用 canonical App Store 链接。
- 摘要必须插入竞品 icon 和竞品宣传图；不要插入自有产品自己的宣传图，除非用户明确要求。
- 远端 `docs +fetch` 必须看到图片 token，或对应对象处写 `无宣传图`。
- 用户可见正文不得出现 `官方 screenshot`、`官方字段`、`字段为空`、`网页回退`、`needs_page_fallback`、`page_fallback`、`lookup`、`scraper`、`接口为空` 等采集管线词。
- manifest 不能停在 `not_started`、`local_only`、`draft_only`、`text_only_created`，除非状态明确为权限、登录、网络或组织策略阻塞。

邮件交付：
- 仅在飞书摘要链接存在并通过验收后执行。
- 使用本地 SMTP 配置 `docs/app-competition/config/smtp.local.json` 和 [send_smtp_mail.py](scripts/send_smtp_mail.py)。
- dry-run 通过后直接发送；缺配置写 `blocked_missing_smtp_config`。

外网访问：
- 用户要求“外网可访问 / public link”时，必须记录 `external_access_status`。
- 不能把未经权限核验的飞书链接称为外网可访问。

## References

- [assets/multi-product-summary-template.md](assets/multi-product-summary-template.md) - 多自有产品摘要模板，必须与 Portfolio Mode 结构一致
- [references/portfolio-summary-format.md](references/portfolio-summary-format.md) - 多产品摘要结构、禁止项和验收门禁
- [references/keyword-source-policy.md](references/keyword-source-policy.md) - 关键词来源优先级、fallback 与 provenance
- [references/file-contracts.md](references/file-contracts.md) - 项目文件职责与字段约定
- [references/evidence-rules.md](references/evidence-rules.md) - 证据、时间戳、素材与变化链规则
- [references/creative-asset-delivery.md](references/creative-asset-delivery.md) - 宣传图下载、留证与飞书插图规则
- [references/link-quality-gate.md](references/link-quality-gate.md) - App Store URL canonical 化与飞书链接语义验收
- [references/e2e-competition-to-feishu.md](references/e2e-competition-to-feishu.md) - 端到端飞书交付门禁
- [references/mail-delivery.md](references/mail-delivery.md) - SMTP 邮件交付
- [references/mcp-installation.md](references/mcp-installation.md) - MCP 安装与验证
- [references/plain-field-check-mode.md](references/plain-field-check-mode.md) - 用户要求只检测字段变化时读取
- [scripts/bootstrap_app_competition.ps1](scripts/bootstrap_app_competition.ps1) - 依赖自修复入口
