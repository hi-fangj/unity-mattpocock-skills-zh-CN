Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=grill-with-docs
```

```bash
npx skills update grill-with-docs
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/grill-with-docs)

## What it does

`grill-with-docs` 围绕一个计划或设计，一次一个问题地持续追问你，直到你和 agent 达成共同的理解——并且它会在进行过程中把词汇和决策写下来。

这场 grilling **会留下书面痕迹**。一次普通的访谈会打磨你的思考，然后在会话结束时蒸发殆尽；而这一个会在每个术语被敲定的那一刻就把它捕获进一份 `CONTEXT.md` glossary，并把那些艰难的、单向的决策记录为 ADRs。这份共识得以在对话之后存续，而不是只活在你的脑子里。

## When to reach for it

你通过输入 `/grill-with-docs` 来调用它——agent 不会自行取用它。

在一次变更的最开始就使用它，那时计划还很模糊、领域语言也尚未敲定，而你想在任何代码存在之前就对两者做压力测试。如果你只想要访谈、不需要那些产物，用 [grilling](https://aihero.dev/skills-grilling)；如果计划已经清晰、你只需要敲定或记录术语，用 [domain-modeling](https://aihero.dev/skills-domain-modeling)。而如果这次变更大到一次会话装不下、它的路线仍然迷雾重重——一个 greenfield 项目、一次庞大的 feature 构建——就从上游的 [wayfinder](https://aihero.dev/skills-wayfinder) 开始：它把这项 effort 绘制成一张决策地图，然后在路径清晰之后交还给这条 main flow。

## Prerequisites

这个 skill 是有状态的——它在 grilling 的同时写入你的 repo。已敲定的术语落入根目录的一份 `CONTEXT.md` glossary（或者，如果一个 `CONTEXT-MAP.md` 标记了多 context 仓库，则落入相关 context 的 `CONTEXT.md`），而真正难以逆转的决策则作为 ADRs 落入 `docs/adr/` 之下。两者都是惰性创建的——在第一个术语或决策成形之前什么都不存在——所以你不需要事先搭建任何脚手架，但你确实需要身处一个可以安全写入这些文件的地方。

## The grill

引擎是一个 **grill**：沿着决策树 relentless 地、一次一个问题地行进，在继续之前先解决决策之间的依赖，并为每个问题提供一个推荐答案。codebase 能回答的问题，会通过阅读 codebase 来回答，而不是来问你。

让这个变体自成一个 skill 的，是那些答案的去向。随着 grill 的进行，模糊的语言被打磨成规范术语，并内联写入 glossary——而不是攒到最后批量写。glossary 始终保持为 glossary：纯粹的词汇，没有实现细节，没有 spec。ADR 被节制地提供，只有当一个决策难以逆转、没有上下文就会令人意外、并且是真正权衡的结果时才提供。大多数会话产出一份更锐利的 glossary 和很少或没有 ADR，而那正是预期的形态。

## It's working if

- 它一次问一个问题并等待，而不是倾倒一份问卷。
- 术语在被敲定的那一刻就写入 `CONTEXT.md`，用的是你项目自己的措辞。
- 在可能的地方，它深入 codebase 去回答它自己的问题。
- ADR 保持稀少——你不会被迫去橡皮图章式地批准那些可逆的选择。

## Where it fits

`grill-with-docs` 是 main build chain 的开场步骤：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

它排在最前，在任何东西被写成 spec 之前：它产出共同的理解和敲定的词汇，[to-spec](https://aihero.dev/skills-to-spec) 随后将其合成为一份 spec，而无需重新访谈你。它亲近的邻居是 [grilling](https://aihero.dev/skills-grilling)——同一场访谈但不带文档，以及 [domain-modeling](https://aihero.dev/skills-domain-modeling)——它所驱动的那套 glossary-and-ADR 纪律。当你不确定哪个 skill 或 flow 契合时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
