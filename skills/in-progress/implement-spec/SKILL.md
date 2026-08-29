---
name: implement-spec
description: "把一份 spec 实现为代码。"
disable-model-invocation: true
---

你已经拿到一份 spec。这份 spec 应该有关联的 tickets，描述如何实现它。

目标是产出一个 PR，在单一分支上实现整个 spec。

这些 tickets 不是步骤清单，而是一张带有 blocking 关系的 **task graph**。这意味着总是存在一个随时可领取的 **frontier**。

与 subagent 的通信应当稀疏。主要通过 **context pointers** 沟通：指向 spec、tickets、research notes 和之前的 commits。不要重复 pointer 已经能获取的信息。

**implementer subagents** 应尽可能放到后台运行，以获得**最大并发**。

## Steps

1. 阅读 spec 和 tickets，读到足以理解 task graph 的程度。

2. （可选）使用一个 **exploration subagent** 完成 tickets 要求的探索：相关代码文件或外部文档。确保 exploration subagent 能保存文件：把它 markdown 笔记保存到 repo 之外的一个目录，让后续所有 subagent 都能访问。这样 **implementer subagents** 就能专注于实现，而不是探索。

3. 创建分支和 draft PR。PR 应标记为 closing 对应的 spec issue 和 tickets。

4. 用 **implementer subagents** 实现每个 ticket。每个 implementer subagent 应在自己的 worktree、自己的分支上工作。

5. 一个 **implementer subagent** 完成后，用 **merger subagent** 把它的工作合并到 PR 分支。

6. 如果可用 tickets 的 **frontier** 发生变化，启动更多 **implementer subagents** 去处理新 ticket，以获得最大并发。

7. 所有 tickets 完成后，在 PR 分支上运行 /code-review。用一个 **implementer subagent** 一次性修复 code review 提出的全部问题。

8. 把 PR 标记为 ready for review。

9. 清理所有 **implementer subagent** 的 worktrees。
