# MCP Playbook

## Server Roles

### `appstore-mcp`

适合：
- iOS 搜索与快速核验
- iOS 榜单与类目观察
- iOS 截图与发布日期补充

优先工具：
- `search_apps`
- `get_trending_apps`
- `get_app_info(include=basic|release|screenshots|full)`

### `mcp-appstore`

适合：
- iOS + Android 双平台搜索
- 相似 app 扩散
- 版本、价格、开发者、评论、关键词分析
- 深度报告的主体数据

优先工具：
- `search_app`
- `get_app_details`
- `get_similar_apps`
- `get_version_history`
- `analyze_reviews`
- `fetch_reviews`
- `get_pricing_details`
- `get_developer_info`
- `analyze_top_keywords`
- `get_keyword_scores`

## Recommended Division Of Labor

- discovery：
  - 先用 `mcp-appstore` 建候选池
  - 再用 `appstore-mcp` 补 iOS 搜索、榜单与截图核验
- reporting：
  - 以 `mcp-appstore` 为主体
  - 用 `appstore-mcp` 补 iOS 截图、发布日期和快速对照

## Limitations To Surface

- `get_keyword_scores` 是近似评分，不是官方精确排名历史。
- `get_version_history` 在部分平台上只能稳定返回最新版本。
- 截图 URL 可以证明“变了”，但不能证明“完全没变”。
