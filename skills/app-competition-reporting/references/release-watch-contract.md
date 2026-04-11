# Release Watch Contract

## Required Fields

每个 app 至少记录：
- `app_name`
- `app_id`
- `app_store_url`
- `current_version`
- `updated_at`
- `within_30d`
- `whats_new_raw`
- `whats_new_summary`
- `release_theme`
- `user_visible_change`

## Writing Rules

- 正文表格默认显示可点击 `app_name`：`[app_name](app_store_url)`
- `app_id` 只保留在 JSON 和附录
- `whats_new_summary` 用业务语言概括，不照抄字段
- `user_visible_change` 建议用 `none / low / medium / high`
- 摘要层不要直接暴露 `maintenance` 等原始分类标签，改写成业务中文

## Interpretation Rules

- `whats_new_raw` 是事实
- `whats_new_summary` 是受控摘要
- `release_theme` 是分类，不是结论
- 策略判断单独写在表后，不要塞进事实表

## Allowed Themes

- `new_capability`
- `quality_or_performance`
- `pricing_or_packaging`
- `seasonal_or_marketing`
- `maintenance`

## Common Cases

- 只写 `bug fix / improve performance`：通常记为 `maintenance` 或 `quality_or_performance`
- 提到新模型、新入口、新功能：可记为 `new_capability`
- 提到优惠、套餐、试用：记为 `pricing_or_packaging`

## Anomaly-First Rule

- 正文 `Release Watch` 只优先写“有变化且有意义”的对象
- 单纯 maintenance 更新默认下沉到详细层或附录
- 如果某对象没有显著发版变化，但会改变判断，可以保留并说明原因
- 不要让一整屏 maintenance 更新抢走真正重要的变化

## Decision Prompt

表后至少回答：
1. 发生了什么
2. 为什么重要
3. 我们该做什么
