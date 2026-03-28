# Release Watch Contract

## Required Fields

每个 app 至少记录：
- `app_name`
- `app_id`
- `current_version`
- `updated_at`
- `within_30d`
- `whats_new_raw`
- `whats_new_summary`
- `release_theme`
- `user_visible_change`

## Writing Rules

- 正文表格默认显示 `app_name`
- `app_id` 只保留在 JSON 和附录
- `whats_new_summary` 用业务语言概括，不照抄字段
- `user_visible_change` 建议用 `none / low / medium / high`

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

## Decision Prompt

表后至少回答：
1. 发生了什么
2. 为什么重要
3. 我们该做什么
