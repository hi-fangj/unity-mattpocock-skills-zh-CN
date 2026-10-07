# Issue tracker 集成：仅支持 GitHub、GitLab 与本地 markdown

`setup-matt-pocock-skills` 提供一等支持的 issue tracker 恰好三种，对应 `skills/engineering/setup-matt-pocock-skills/` 中的每个 seed template：

- **GitHub**（`issue-tracker-github.md`，通过 `gh`）
- **GitLab**（`issue-tracker-gitlab.md`，通过 `glab`）
- **本地 markdown**（`issue-tracker-local.md`，`.scratch/` 下的文件）

为任何其他 tracker 新增一等后端的请求不在范围内。这包括 Jira、Linear、Azure DevOps、Trello、YouTrack，以及新的面向 agent 的 tracker。

## 为什么这不在范围内

每个 issue-tracker 后端都会把一种 CLI 形态硬编码进 skills（命令、flag、输出解析）。每新增一个后端，都是永久维护面：它必须随着工具 CLI 的演进继续可用，也必须持续针对 `/to-spec`、`/to-tickets`、`/triage` 等工作流测试。作者自己用的是 GitHub，所以那是唯一能保持正确的后端。GitLab 形态足够接近，可以顺带维护；本地 markdown 则完全不需要 CLI。

流行度不改变这一点。Jira 用户很多，也有官方 CLI（`acli`），但它依然不会获得 template：那是一个作者自己不运行的后端，有自己的一套链接语义、workflow states 和 CLI quirks 需要持续测试。无论工具是否主流，成本都落在作者身上。

所有其他 tracker 都通过 `/setup-matt-pocock-skills` 的 **Other** 选项接入：你用一段话描述自己的 workflow，skill 会把它作为 prose 记录进 `docs/agents/issue-tracker.md`。这个文件归你所有。如果你打磨出了可用的 Jira 或 Linear workflow，把它保存在自己的 repo 里或发布出来，并让 setup 指向它。

## Prior requests

- #99: "Add dex as an issue tracker backend"（dex 当时约 3 个月大，约 300 stars）
- #258: "Add Jira as first-class issue tracker via Rovo MCP"
- #1137: "Add Jira as a first-class issue tracker, via Atlassian's official CLI (`acli`)"
