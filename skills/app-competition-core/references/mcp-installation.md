# MCP Installation

## When To Use

出现以下任一情况时，先用这份说明，不要直接进入 discovery 或 reporting：
- 用户说“还没装好 MCP”
- 工具调用报错，说明服务器不存在或不可用
- 新环境第一次跑竞品分析

## Required Servers

竞品分析默认依赖两套 MCP：
- `appstore-mcp`
- `mcp-appstore`

分工说明见 [mcp-playbook.md](mcp-playbook.md)。

## Install `appstore-mcp`

### Option A: `npx` 方式

```bash
npx appstore-mcp
```

Codex / MCP 配置示例：

```toml
[mcp_servers."appstore-mcp"]
command = "npx"
args = ["appstore-mcp"]
```

### Option B: 本地 `node` 方式

如果已经把包放到本地，可直接指向 `dist/index.js`：

```toml
[mcp_servers."appstore-mcp"]
command = "node"
args = ["E:/codex/vendor_imports/mcp/appstore-mcp/node_modules/appstore-mcp/dist/index.js"]
```

## Install `mcp-appstore`

```bash
git clone https://github.com/appreply-co/mcp-appstore.git
cd mcp-appstore
npm install
node server.js
```

Codex / MCP 配置示例：

```toml
[mcp_servers."mcp-appstore"]
command = "node"
args = ["E:/codex/vendor_imports/mcp/mcp-appstore/server.js"]
cwd = "E:/codex/vendor_imports/mcp/mcp-appstore"
```

## Codex Reference Config

在当前这套本地环境里，MCP 配置参考文件是：
- `E:/codex/config.toml`

当前配置形态：

```toml
[mcp_servers."appstore-mcp"]
command = "node"
args = ["E:/codex/vendor_imports/mcp/appstore-mcp/node_modules/appstore-mcp/dist/index.js"]
startup_timeout_sec = 20
tool_timeout_sec = 60

[mcp_servers."mcp-appstore"]
command = "node"
args = ["E:/codex/vendor_imports/mcp/mcp-appstore/server.js"]
cwd = "E:/codex/vendor_imports/mcp/mcp-appstore"
startup_timeout_sec = 20
tool_timeout_sec = 120
```

## Minimum Verification Checklist

安装或改配置后，至少验证以下 4 个工具可以调用：
- `appstore-mcp.search_apps`
- `appstore-mcp.get_app_info`
- `mcp-appstore.search_app`
- `mcp-appstore.get_app_details`

验证通过后，再进入正式的 discovery 或 reporting。

建议的最小 smoke test 口令：
- “搜索美国区的 photo editor apps”
- “读取某个 iOS app 的 release 信息”
- “搜索 ios 平台的 remove watermark”
- “读取某个 app 的详情页和截图字段”

## Failure Handling

- 缺一个 MCP：先修安装，不做半残报告。
- `appstore-mcp` 不可用：iOS 搜索、截图、发版补充会受影响。
- `mcp-appstore` 不可用：关键词、评论、开发者、价格、相似应用分析会受影响。
- 两个都不可用：直接停下，不进入竞品分析。
