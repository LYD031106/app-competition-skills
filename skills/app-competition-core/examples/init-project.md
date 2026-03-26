# Example: Initialize A Project

用户请求：

```text
帮我给这个项目建立 app 竞品分析目录，需要 app-compete.md 和 focus-apps.md，默认先看美国市场。
```

预期动作：
- 创建 `docs/app-competition/`
- 创建 `app-compete.md`
- 创建 `focus-apps.md`
- 创建 `reports/`、`snapshots/`、`evidence/`
- 先检查 MCP 是否可用；若用户还没装，先引用安装说明
- 若没有额外要求，可把模板里的示例市场设为 `US`
- 明确提醒用户补充 `target_app`

不该做的事：
- 不要在用户未确认时把 `CN` 当成默认市场
- 不要先写任何竞品报告
