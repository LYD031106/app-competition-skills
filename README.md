# App Competition Skills

一组用于应用竞品分析的可复用 skills，适合放入 Codex/Claude 等 agent 的 skill 目录中使用。

## 包含内容

- `skills/app-competition-core`
- `skills/app-competition-discovery`
- `skills/app-competition-reporting`

这三个 skill 按职责拆分：

- `app-competition-core`：初始化竞品分析工作区、维护项目级索引，并路由到 discovery 或 reporting。
- `app-competition-discovery`：发现新增竞品、做候选筛选与 `Track / Watch / Drop` 决策。
- `app-competition-reporting`：生成竞品深度报告、JSON 快照与项目回写信息。

## 目录结构

```text
.
├── README.md
├── .gitignore
└── skills/
    ├── app-competition-core/
    ├── app-competition-discovery/
    └── app-competition-reporting/
```

## 安装

将本仓库中的 `skills/app-competition-*` 目录复制到你的 skills 根目录即可。

Codex 环境示例：

```powershell
Copy-Item -Recurse -Force .\skills\app-competition-* E:\codex\skills\
```

## 使用要求

- 项目内需要有 `docs/app-competition/` 工作区。
- `default_market` 必须在项目文件中显式声明。
- 竞品分析输出应保留证据链，避免无证据结论。

## 远程仓库推送

本仓库已按独立 Git 仓库组织，可直接关联 GitHub 远程后推送。
