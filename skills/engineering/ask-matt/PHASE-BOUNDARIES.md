# Phase boundaries

**Phase** 是 session 内的一块工作——grilling、implementation、QA。这个定义故意模糊：当你想"好了，这部分做完了"时，一个 phase 就结束了。

**Phase boundary** 是两个 phase 之间的空隙，也是这个决定唯一该做的地方。Mid-phase 没有任何决定可做——continue，或把剩下的工作拆给 subagents。Mid-phase compact 会让 agent 丢掉线索。

## 五个选项

| 选项 | 作用 |
| ---- | ---- |
| **Continue** | 留在 session 中。完全没有 context switch。 |
| **`/clear`** | 清空 context window，从零开始。 |
| **`/handoff`** | 写一个 portable markdown file，用它把 session seed 到任何地方。 |
| **Subagent** | 把任务送进它自己的 context window，拿回一份 report。 |
| **`/compact`** | 压缩当前 context，用 summary seed fresh session。 |

## 决策树

在 boundary 处从上到下判断。第一个 **yes** 胜出。

**1. 你能继续留在当前 session 吗？** 两件事会让答案是 yes：下一个 phase 需要这个 phase 作为 **primary source**，或者你的 [smart zone](https://www.aihero.dev/ai-coding-dictionary/smart-zone) 还剩下足够多（约 150k tokens）让下一个 phase 放得下。Grilling → implementation 是标准的 yes：implementation 想要的是逐字的推理过程，而不是它的 summary。Continue 零成本、零损失，所以先于一切排除它。

**2. 当前 context 对接下来要做的事无关紧要吗？** 这个 session 里的一切——探索、决定、死路——都是可丢弃的吗？如果是，就 **`/clear`**。这是棋盘上最便宜的一步：不花时间，把整个 window 交还给你。`/clear` 也不是终点——旧的 session 仍然可以 resume。

把这个判断做错的成本是单向的。Clear 掉一个 _相关的_ context，你就失去了所做之物的**为什么**，再怎么回读 diff 也拿不回来。

**3. 你需要 hand off 吗？** `/handoff` 的用途很窄。只有当你是以下情况时才需要它：

- 换到 **new harness**（Claude → Codex），
- 移动到 **new directory** 或 repo，
- 把工作交给 **colleague**，
- 或 **mid-phase** 分叉一个 side task，而不打乱你正在做的事。

这个列表就是全部条款。`/handoff` 买到的是 **portability**——一份能旅行的文件。如果没有什么在旅行，你就不需要它。

**4. 这个任务可以 AFK 完成吗？** 它是否足够 tightly-scoped，让你离开键盘也能跑完、不需要中途操控？如果是，就把它交给 **subagent**，让这个 session 保持原样。Automated review 是标准场景：agent 读 diff 并汇报，期间不需要你。

**5. 否则，`/compact`。** 相关 context、同一个 harness、同一个 directory，而且你需要留在 loop 中——决策树落到这里，而且经常落到这里。给它一条指令（`/compact we're going to QA this area`），这样 summary 就能保留下一个 phase 需要的东西。

`/compact` 是 **default，不是第一选择**。它坐在树底，因为上面四个问题都比它更便宜或更精确。当人们从这里开始时，failure mode 是一个对 summary 压平的 decision 信心满满、其实错得离谱的 fresh session。

## Primary and secondary sources

除 **Continue** 外的每一步都把 **primary source** 变成 **secondary source**——按原样发生的 session，被它的 summary 取代。权衡总是同一个形状：

| Source | 信息 | 噪音 | 腾挪空间 |
| ------ | ---- | ---- | ---- |
| Primary（Continue） | 完整 | 很多 | 很少 |
| Secondary（`/compact`、`/handoff`） | 有损 | 更少 | 很多 |

这就是为什么问题 1 排在最前面。只有当留在原地比保存内容更贵时，你才付出这份有损。

## 这些都是判断

这些问题并不客观——每个都有品味成分，同一个 boundary 在两天里可能走两个方向。价值在于**按顺序**问它们，在 boundary 处问而不是在工作中途问。
