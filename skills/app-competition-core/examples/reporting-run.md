# Example: Reporting Run

用户请求：

```text
帮我做本周竞品分析，重点看关键词、最近发版和素材变化。
```

预期动作：
- 先读取 `focus-apps.md`
- 若有历史快照则做 delta；没有则标记 baseline
- 生成 Markdown 报告
- 生成 JSON 快照
- 更新 `app-compete.md` 的 `report_history`
- 更新涉及 app 的 `last_reviewed_at`

不该做的事：
- 没有历史时不要伪造趋势
- 不要只产出 Markdown
