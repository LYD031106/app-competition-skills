# Example: Reporting Run

用户请求：

```text
帮我做美国区 iOS 月度竞品深度报告，重点看最近 30 天发版、前三张宣传图变化和高流量词排名波动。
```

预期动作：
- 先确认 `default_market=US`
- 先确认两套 MCP 已安装且可调用
- 报告类型使用 `monthly-deep-dive`
- 先读取 `focus-apps.md`
- 若有历史快照则做 delta；没有则标记 baseline
- 生成 Markdown 报告
- 生成 JSON 快照
- 在 JSON 中写入 `report_type`、`comparison_window`、`keyword_analysis.rank_tracking`、`release_analysis.apps`、`material_analysis.apps`
- 更新 `app-compete.md` 的 `report_history`
- 更新涉及 app 的 `last_reviewed_at`

不该做的事：
- 没有历史时不要伪造趋势
- 不要把 `What's New`、前三张截图变化或关键词 rank 告警漏掉
- 不要只产出 Markdown
