# Report Rendering Style

## Audience

默认读者是产品 / ASO 负责人。写法必须支持“3 分钟可扫读”。

## Name-First Rule

- 正文默认只写 `app_name`
- 主表格默认只显示名称，不显示 `app_id`
- 首次出现可用“完整名称 + 角色短标签”
- 名称非常接近时，优先用“名称 + 角色 / 开发者 / 定位短标签”区分
- `app_id` 只保留在附录、JSON、证据备注和必要的歧义说明

## Structure Rule

报告优先级高于元数据：
- 结论在前
- 三问执行摘要在前
- 证据附录在后
- 次市场后置

## Table Rule

窄表优先：
- 表内放事实
- 表后放推断
- 单元格避免整段话
- 列名尽量中文化、业务化

## Tone Rule

- 用研究口吻，不用 API 转抄口吻
- 不要把工程字段堆在首屏
- 不要让 `report_type`、`comparison_window`、`baseline_ref` 抢首屏注意力

## Limitation Rule

- 局限不只放文末
- 如果某个模块依赖推断或缺历史资产，在模块旁即时提示

## Good First Screen

首屏通常只需要：
- 3-5 条 TL;DR
- 1 张三问执行摘要表
- 1 张主市场比较矩阵
