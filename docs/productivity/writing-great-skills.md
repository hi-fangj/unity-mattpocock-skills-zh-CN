Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=writing-great-skills
```

```bash
npx skills update writing-great-skills
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/writing-great-skills)

## What it does

`writing-great-skills` 是你编写和编辑 skills 时所对照的 reference——那套让 skill 变得可预测的共享词汇与原则。

一个 skill 的职责是从一个随机系统中驯服出确定性，所以目标不是每次运行都有相同的*输出*，而是相同的*过程*。**Predictability** 是根本美德，每一个设计选择都要对照它来评判——而不是对照这个 skill 读起来有多聪明、多完整或多详尽。

## When to reach for it

你通过输入 `/writing-great-skills` 来调用它——agent 不会自行调用。

每当你在创作一个新 skill 或编辑一个既有 skill、并希望它每次都以同样的方式表现时，就用它：决定 invocation mode、撰写 description、选择什么放在 `SKILL.md` 里而什么放在链接的文件中，或者诊断一个 skill 为什么会失灵。

## Cognitive load

整份 reference 所围绕的核心概念是 **cognitive load**——以及它的对应面 **context load**。每个 skill 都要花费其中一种：

- 一个 **model-invoked** skill 每一轮都在窗口中保留一份 description，因此它花费 **context load**，但能自行触发。
- 一个 **user-invoked** skill 剥掉了那份 description；它花费零 context load，但这样一来*你*就成了那个必须记得它存在的索引——这就是 **cognitive load**。

这些 skills 中的大多数是 user-invoked 的，这正是 cognitive load 成为整个系统被构建出来要管理的压力的原因：当 user-invoked skills 多到超出你能记在脑子里的程度时，解药就是一个 **router skill**，它点名其他 skills 以及何时调用每一个。一旦你开始用这两种 load 来思考，大多数创作决策——拆还是不拆、内联还是披露、model-invoked 还是 user-invoked——就变成了在不同地方做出的同一个权衡。

## The other levers

这份 reference 的其余部分，是把那些 load 花得恰到好处的工具箱：

- **Leading words**——一个已经存在于模型预训练中的紧凑概念（_tight_、_red_、_tracer bullet_），agent 在运行 skill 时用它来思考。它用最少的 tokens 同时锚定执行*和*调用；去搜寻那些一个词就能取代的赘述。
- **Information hierarchy**——从 skill 内的步骤，到 skill 内的 reference，再到藏在 **context pointer** 之后的外部 reference 的阶梯。**Progressive disclosure** 就是沿着那架阶梯往下走的动作，好让顶层保持清晰可读。
- **Pruning**——single source of truth、相关性，以及逐句施加的 no-op test，用以对抗 **sediment** 和 **sprawl**。
- **Failure modes**——**premature completion**、**duplication**、**sediment**、**sprawl**、**no-op**——用来诊断一个行为失常的 skill。

## Where it fits

这是一个随时可调用的 standalone reference——你在构建其余 skills 时所查阅的 meta-skill，而不是链条中的一个步骤。它天然的邻居是你所维护的任何 router，因为 router 正是 user-invoked skills 堆积起来的 cognitive load 的直接解药；当你不确定哪个 skill 或流程适合某项任务时，[ask-matt](https://aihero.dev/skills-ask-matt) 会在整个集合之上为你路由。
