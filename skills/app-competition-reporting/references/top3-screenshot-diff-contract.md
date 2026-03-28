# Top-3 Screenshot Diff Contract

## Scope

永远只盯前三张图，因为它们最影响用户首屏判断。

## Required Fields

每个 app 至少记录：
- `app_name`
- `app_id`
- `top3`
- `top3[].position`
- `top3[].semantic_summary`
- `top3[].inferred`

每次变化判断至少记录：
- `changed`
- `before_theme`
- `after_theme`
- `message_shift`
- `likely_reason`
- `change_chain`

## Change Priority

按这个顺序判断是否变化：
1. URL 变化
2. 顺序变化
3. 主题变化

## Rendering Rules

正文主表只保留：
- `竞品名称`
- `前三屏主线`
- `是否变化`
- `对我方启发`

不要把 `likely_reason`、`change_chain`、长解释塞进表格。

## Evidence Rules

- 直接读取到截图或可靠标题：`inferred=false`
- 只根据 URL、描述或素材顺序做语义判断：`inferred=true`
- 没有历史截图时：写 `current baseline`，不要假装有 diff
- 没有截图时：明确写素材缺口，不强行做语义分析

## Interpretation Rules

- `likely_reason` 必须标为推断
- `change_chain` 只有与发版、关键词、标题变化等证据能串起来时才写
- 单个信号不足时，把结论降级为可能性判断
