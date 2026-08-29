Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=domain-modeling
```

```bash
npx skills update domain-modeling
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/domain-modeling)

## What it does

`domain-modeling` 在你做设计的同时，构建并打磨一个项目的 **ubiquitous language**：挑战模糊的术语，用具体场景对关系做压力测试，并在术语和决策成形的那一刻就把它们写下来。

这是**主动的**纪律，而不是被动的。仅仅读 `CONTEXT.md` 来借用它的词汇，是任何 skill 都能做到的一行习惯；这个 skill 用于你正在*改变*模型的时候：确立一个规范术语、捕捉代码与你刚说的话之间的矛盾、记录一个难以逆转的决策。它还保持 glossary 的干净：`CONTEXT.md` 就是一份 glossary，别无其他，没有实现细节，没有 spec，没有草稿本。

## When to reach for it

输入 `/domain-modeling`，或者当任务契合时由 agent 自动调用：当你在敲定术语、解决一个被过度使用的词，或记录一个架构决策时。

当问题出在*词语*上时使用它：两个人对 "cancellation" 的理解不同，"account" 在干三份工作，或者一场设计对话总是卡在一个从未被精确命名过的概念上。如果问题出在 module 的*形状*上（seam 放在哪里、interface 有多深），用 [codebase-design](https://aihero.dev/skills-codebase-design)。如果你想让计划本身在构建之前被审问，用 [grilling](https://aihero.dev/skills-grilling)。

## Prerequisites

这个 skill 写入两个地方，两者都是惰性创建的：只有在有东西要记录时才创建。已敲定的术语进入根目录的 `CONTEXT.md`（或者，在一个由 `CONTEXT-MAP.md` 标记的多 context 仓库中，进入各自 context 的 `CONTEXT.md`）。决策进入 `docs/adr/`。事先什么都不需要存在；第一个敲定的术语创建 glossary，第一个真正的权衡创建 ADR。

## Glossary vs. ADR

两种产物，两种不同的门槛：

- **glossary**（`CONTEXT.md`）捕捉语言。每当一个模糊的术语被确立为规范，它就内联写下来（不是攒到最后批量写），这样共享词汇就能与对话保持同步。它毫不留情地不沾实现细节。
- **ADR** 捕捉一个决策，而门槛很高：只有当这个选择**难以逆转**、**没有上下文就会令人意外**、并且**是真正权衡的结果**时才提供。三者缺一就没有 ADR。正是这一点让 `docs/adr/` 保持为重大分岔的记录，而不是一本日记。

让它运转起来的那一步：当你陈述某样东西如何运作时，这个 skill 会交叉引用代码并浮现出矛盾：「你的代码取消的是整个 Order，但你刚说部分取消是可能的，哪个是对的？」语言和代码被迫达成一致。

## Pulled out on purpose

`domain-modeling` 是构建项目 ubiquitous language 的**唯一权威来源**，它被拆出来作为自己的 model-invoked skill，这样任何其他 skill 都能触达它。[grill-with-docs](https://aihero.dev/skills-grill-with-docs) 依赖它在 grilling 会话进行时记录术语和决策，[triage](https://aihero.dev/skills-triage) 用它让 tickets 保持使用项目自己的措辞，而 [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) 在工作时会调用它。

保持它 standalone 意味着你也可以直接调用它，作为关于如何打磨一个模型的**参考**，而不必承诺那些 skills 所要求的步骤。语言存放在一个地方，而一切需要它的东西都指向那里。

## Where it fits

`domain-modeling` 是一个**随时可调用的 standalone**，它*位于*其他 skills 之下运行的频率，与在固定步骤上运行一样高。它最接近的邻居是 [codebase-design](https://aihero.dev/skills-codebase-design)，因为共享语言正是让你能精确命名一个 deep module 及其 seam 的东西；往下游，一份敲定的 glossary 正是 [to-spec](https://aihero.dev/skills-to-spec) 合成为一份用项目自己的措辞写成的 spec 的东西。当你不确定哪个 skill 或 flow 契合时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
