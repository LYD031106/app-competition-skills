# Example: First Run After MCP Setup

用户请求：

```text
我还没装好竞品分析要用的 MCP，先帮我配好，再做一份美国市场的竞品月报。
```

预期动作：
- 先暂停 discovery / reporting
- 引用 `references/mcp-installation.md`
- 说明需要安装的两个 MCP：`appstore-mcp`、`mcp-appstore`
- 给出配置示例，并提示检查 `E:/codex/config.toml`
- 明确最小验证集：`search_apps`、`get_app_info`、`search_app`、`get_app_details`
- 验证通过后，再进入正式 reporting 流程

不该做的事：
- 不要在 MCP 不可用时硬做降级版报告
- 不要跳过验证，直接假设安装成功
