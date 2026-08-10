Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=teach
```

```bash
npx skills update teach
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/teach)

## What it does

`teach` 把当前目录变成一个常设的教学 workspace，并在多个 sessions 中教你一个主题——设计出简短、优美、可交互的课程，并与你*为什么*想学绑定在一起。

它**不会**从模型自己的记忆中教学。参数化知识被视为不可信；在能够教学之前，它会收集高可信度的资源，并把每一个论断都锚定在一个引用上。而且它是 stateful 的——workspace 会记住你学过什么，因此每个 session 都从上一个停下的地方继续，而不是从零开始。

## When to reach for it

你通过输入 `/teach` 来调用它——agent 不会自行调用。

当你想随着时间*学习*一个主题——一门语言、一个框架、瑜伽、理论物理——并希望 sessions 能够累积而不是蒸发时，就用它。它不是为一次性的解释准备的；如果你只是需要当场把某件事弄清楚，直接问就好。当学习是一个项目时，才调用 `teach`。

## Prerequisites

`teach` 会在原地构建一整个目录，所以请在一个你乐意长期保留为专用 workspace 的地方运行它。随着时间推移，它会写出：

- `MISSION.md`——你学习它的原因，它为其他一切提供 grounding。如果它是空的，`teach` 的第一件事就是追问你，直到它不再为空。
- `RESOURCES.md`——经过审定的、高可信度的来源，它据此教学。
- `./lessons/*.html`——编号的、自包含的课程（教学的主要单元）。
- `./reference/*.html`——压缩的 cheat-sheets、算法、你会反复回看的 glossaries。
- `./learning-records/*.md`——你学过的内容，ADR 风格，用来判断接下来教什么。
- `./assets/*`——可复用的组件（首先是一份共享的 stylesheet），好让课程看起来像同一门课。
- `NOTES.md`——你的教学偏好。

## Mission, and the zone of proximal development

每一节课都挂在 **mission** 之下。没有它，知识就没有可以附着的东西，课程也会显得抽象——所以 mission 是 `teach` 首先确定下来的东西，并会随着你的成长不断更新。从 mission 和你的 learning records 出发，它计算出你的 **zone of proximal development**（最近发展区）：下一节课应当*恰好*挑战你到足够的程度，不多也不少。

## Storage strength, not fluency

用来思考的词是 **storage strength**——长期保持——与之相对的是 **fluency**，那种当下的、感觉像精通但其实不是的即时回忆。`teach` 通过 desirable difficulty 刻意地构建前者：retrieval practice、spacing 和 interleaving。知识先被教授（在这里难度是敌人），然后 skills 通过一个紧密的反馈 loop 被反复操练（在这里难度是工具）。

## Where it fits

`teach` 是一个随时可调用的 standalone——一个由你逐个 session 驱动的长期学习项目，而不是 build chain 中的一个步骤。它与其他 productivity skills 不共享任何 workflow；它只是拥有自己的 workspace 目录并驻留其中。当你不确定哪个 skill 或流程合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
