# Top-3 Screenshot Diff Contract

## Scope

永远只盯前三张图，因为它们最影响用户首屏判断。

## Required Fields

每个 app 至少记录：
- `app_name`
- `app_id`
- `app_store_url`
- `top3`
- `top3[].position`
- `top3[].semantic_summary`
- `top3[].inferred`
- `screenshot_collection_status`
- `screenshot_sources`
- `device_coverage`
- `webpage_screenshot_evidence`
- `web_fallback_used`

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

截图采集顺序固定为：
1. `appstore-mcp.get_app_info(include=screenshots)`
2. `mcp-appstore.get_app_details`
3. Apple 页面 HTML 回退

`screenshot_collection_status` 取值固定为：
- `tool_present`
- `tool_empty_web_present`
- `tool_empty_web_empty`
- `unresolved`

`device_coverage` 取值固定为：
- `iphone`
- `ipad`
- `mixed`
- `unknown`

## Rendering Rules

正文主表只保留：
- `竞品名称`
- `前三屏主线`
- `是否变化`
- `对我方启发`

`竞品名称` 使用 `[app_name](app_store_url)`，不要在正文主表暴露 `app_id`。

不要把 `likely_reason`、`change_chain`、长解释塞进表格。

## Evidence Rules

- 直接读取到截图或可靠标题：`inferred=false`
- 只根据 URL、描述或素材顺序做语义判断：`inferred=true`
- 没有历史截图时：写 `current baseline`，不要假装有 diff
- 没有截图时：明确写素材缺口，不强行做语义分析
- `og:image`、`share banner`、`Placeholder.mill` 不算 `top3 screenshots`
- 只有工具和网页回退都为空时，才能写 `tool_empty_web_empty`

## Interpretation Rules

- `likely_reason` 必须标为推断
- `change_chain` 只有与发版、关键词、标题变化等证据能串起来时才写
- 单个信号不足时，把结论降级为可能性判断
- `MCP 未返回截图` 是工具结论，不是 storefront 结论
- 如果状态是 `tool_empty_web_present`，正文必须写成“工具未返回截图，但网页回退检测到截图”
- 如果状态是 `tool_empty_web_empty`，正文才允许写“App Store 页面无截图”
