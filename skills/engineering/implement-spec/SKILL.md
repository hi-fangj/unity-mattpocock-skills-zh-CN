---
name: implement-spec
description: "把 /to-spec 和 /to-tickets 的结果实现为代码。"
disable-model-invocation: true
---

你已拿到一份 spec。这份 spec 应该附带相关 tickets，说明如何实现这个 spec。

Issue tracker 应该已经提供给你。如果没有，告诉用户运行 `/setup-matt-pocock-skills`。

目标是把整个 spec 实现在一条 **integration branch** 上，并按 issue tracker 收尾工作的方式 resolve 每个 ticket。

Tickets 不是一组步骤，而是一张带 blocking relationships 的 **task graph**。这意味着总是存在一个 **frontier**：已就绪、可以被领取的 tickets。

与 subagents 的往返沟通应当精简，主要通过 **context pointers** 交流：指向 spec、tickets、research notes 和已有 commits。不要复述 pointers 已经提供的信息。

**Implementer subagents** 应尽可能在后台运行，以获得最大并发。

## Steps

1. 阅读 spec 和 tickets，理解 task graph。

2. （可选）使用一个 **exploration subagent** 完成 tickets 所需的探索：相关的 codebase 文件或外部文档。确保 exploration subagent 能保存文件；把它产生的 markdown notes 保存到 repo 之外、后续所有 subagents 都能访问的目录。这样 **implementer subagents** 就能专注于实现而不是探索。

3. 创建 integration branch。如果 issue tracker 通过 PR 收尾工作，或用户要求 PR，在步骤 5 的第一次 merge 之后打开一个 draft PR（领先 main 零 commit 的 branch 无法开 PR），并标记为 closing 对应 spec 和 tickets。

4. 用 **implementer subagents** 实现每个 ticket，每个 subagent 在自己的 worktree、自己的 branch 上工作。每个 implementer subagent：
   - 开始前确认自己的 worktree 基于 integration branch，不是则 reset 到它之上；
   - 调用 Skill tool 运行 `tdd` 构建 ticket；
   - 报告完成前，先把 integration branch 的 tip merge 进自己的 branch。

5. 一个 **implementer subagent** 完成后，用一个 **merger subagent** 把它的工作 merge 到 integration branch。

6. 如果这改变了可用 tickets 的 **frontier**，为新 tickets 再启动更多 **implementer subagents**，以获得最大并发。

7. 所有 tickets 完成后，对 integration branch 调用 Skill tool 运行 `code-review`。用一个 **implementer subagent** 修复 code review 提出的全部问题。

8. 如果存在 draft PR，把它标记为 ready for review。否则按 issue tracker 收尾工作的方式 resolve 每个 ticket，并报告 integration branch。

9. 清理所有 **implementer subagent** 的 worktrees。
