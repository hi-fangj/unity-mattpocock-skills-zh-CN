Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=implement-spec
```

```bash
npx skills update implement-spec
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/implement-spec)

## What it does

`implement-spec` 拿到一份 [spec](https://www.aihero.dev/ai-coding-dictionary/spec) 和它的 [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket)，一次运行就把整个 spec 落地。编排 [agent](https://www.aihero.dev/ai-coding-dictionary/agent) 把每个 ticket 交给一个在独立 git worktree 中工作的 implementer [subagent](https://www.aihero.dev/ai-coding-dictionary/subagent)，把完成的 branch 逐个 merge 进一条 **integration branch**，对结果运行 [code-review](https://aihero.dev/skills-code-review)，然后 resolve tickets。

它把 tickets 读作一张 **task graph**，而不是一个列表。Blocking edges 决定什么可以开始，所以任一时刻都存在一个 **frontier**：所有 blockers 都已落地的 tickets，而 frontier 上的每个 ticket 同时开工。这就是它与逐个处理 tickets 的区别：graph 的形状决定节奏，而不是 tickets 在 tracker 上的排列顺序。

## When to reach for it

你通过输入 `/implement-spec` 调用它，agent 不会自行使用它。

| 你的情境 | 用什么 |
| --- | --- |
| 一份已按 blocking edges 拆成 tickets 的 spec，想一次运行落地 | `/implement-spec` |
| 一次一个 ticket，在自己的 [context window](https://www.aihero.dev/ai-coding-dictionary/context-window) 中，tickets 之间 [clear](https://www.aihero.dev/ai-coding-dictionary/clearing) | [implement](https://aihero.dev/skills-implement) |
| spec 还没拆成 tickets | 先用 [to-tickets](https://aihero.dev/skills-to-tickets) |
| 没有 task graph 可言的小改动 | 直接用 [implement](https://aihero.dev/skills-implement) |

前提：一个已由 [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) 配置的 issue tracker（否则它会停下并让你先运行 setup，而不是瞎猜）、带 blocking edges 的 tickets，以及一个能在后台运行 subagents 并给每个 subagent 独立 worktree 的 [harness](https://www.aihero.dev/ai-coding-dictionary/harness)。subagent 一次只能跑一个的 harness 上，它只是一个更慢的 `implement`。

## Integration branch

所有工作落在一条 branch 上。每个 implementer 先确认自己的 worktree 基于 integration branch，用 [tdd](https://aihero.dev/skills-tdd) 一次一个 red-green slice 地构建 ticket，并在报告完成前把 integration branch 的 tip merge 进自己的 branch，让落地成为 fast-forward。

Tracker 决定是否存在 pull request。如果你的 tracker 通过 PR 收尾工作，或你要求一个 PR，skill 会在第一次 merge 后打开 draft PR 并在结束时标记 ready；否则运行结束在 integration branch 上，每个 ticket 按你的 tracker 收尾工作的方式 resolve，对本地 markdown tracker 完全离线可用。

注意两点：`code-review` 只在所有 tickets 都落地之后运行一次（提前运行会把所有未构建的 ticket 读作失败，触发修复循环）；每个 implementer 都驱动 `tdd`，但不会像 `implement` session 那样与你交互式确认 seams，想钉住 seams 就把它们写进 spec 或 tickets。如果看到第二轮大范围 review 启动，让它只对已修复的 findings 跑聚焦检查然后停止。

## Where it fits

`implement-spec` 是 main chain 中 build step 的并行形态：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review → retro
```

[to-tickets](https://aihero.dev/skills-to-tickets) 产出的 blocking edges 既是逐 ticket 运行 [implement](https://aihero.dev/skills-implement) 的顺序依据，也是 `implement-spec` 的 task graph。它的收尾 [code-review](https://aihero.dev/skills-code-review) 与逐 ticket 形态相同，只是对象是整条 integration branch。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
