# Child Doc Contract

## Goal

详细子文档保留当前完整报告结构，但前部必须先提供决策入口层。

它的职责不是再压缩一轮，而是：
- 承接主摘要里的结论
- 保留完整 `keyword / name-title / screenshot / release / evidence` 内容
- 给需要继续深挖的人足够细节

## Preserve Existing Content

- 不删除现有 `Release / Screenshot / Keyword / Evidence` 结构
- 不因为“做了主摘要文档”而把详细分析裁掉
- 允许在正文前新增：判断型标题、摘要入口、主摘要回链、交付说明
- 允许用 `决策入口层 / 研究证据层` 把前后内容分层

## Required Additions

详细子文档默认新增：
- 判断型标题
- `Detail Delivery Context`
- `Back To Main Summary`
- `决策入口层`
- `研究证据层`
- 增加 `一句话结论`
- 增加 `目标产品关键词变化`
- 增加 `竞品动态`
- 增加 `建议行动`

## Writing Rule

- 详细子文档可以长，但仍要决策优先
- 首屏默认不出现 `app_id`、裸 URL、原始状态码和工具内部标签
- 目标 app、竞品和关键词 leader app 首次出现时，优先写成 `[app_name](app_store_url)`
- `fact`、`inference`、`limitation` 继续分开写
- 如果截图来自网页回退，要明确写出“工具抓取存在覆盖缺口”
- 首屏动作不要和后文结论重复堆叠
