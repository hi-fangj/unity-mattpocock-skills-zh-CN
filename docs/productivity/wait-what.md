Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=wait-what
```

```bash
npx skills update wait-what
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/wait-what)

## What it does

`wait-what` 是当一条消息没有落地时你输入的东西。[agent](https://www.aihero.dev/ai-coding-dictionary/agent) 随后重新表述它刚才说的话。它补上你缺失的 context，用平实的英语书写，并使用你项目 `CONTEXT.md` 里的词汇。

这个 skill 只有三行。那是设计，不是未完成的草稿。与啰嗦作斗争的 skills 会因变长而失败：一份四百字的 concision skill 仍然让 [model](https://www.aihero.dev/ai-coding-dictionary/model) 啰嗦，因为 model 读的是篇幅，而不是恳求。这一个只携带一个精确的 leading word，别无其他。

## When to reach for it

输入 `/wait-what` 调用它。Agent 不会自己伸手用它，也不该用它。只有你知道自己什么时候跟不上。

一旦你注意到自己在扫读，就立刻使用它。Agent 已经漂移进它自己发明的 jargon、叠了五个 acronyms，或解释了一个你从未见过前提的 decision。它修复你已经在进行的对话。要阻止 jargon 从一开始就出现，用 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)，它提前构建 shared language。

## The name is the mechanism

Leading word 是 **wait**。"Be concise" 是关于 agent output 的指令，model 通过删词来服从它，让你更跟不上。**Wait** 是关于*你*的状态。它说理解在这里失败了。听到 "be brief" 的 agent 写电报；听到 "wait, you lost me" 的 agent 会退回去解释。

那个差别就是整个 skill。每个流行的治啰嗦方案都命名 *output*：`/tldr`、`/no-fluff`、`/talk-normal`。Model 过度纠偏成原始人语域，更短但不更清楚。命名 *listener* 同时要求两半：更少的词 **和** 你缺失的 context。

Skill 说 re-pitch **that**，不是 "that last message"。让你跟丢的东西通常比一段大，所以 agent 决定退多远。

## It plugs into the language you already have

正文复用你全局 `CLAUDE.md` 和你项目 `CONTEXT.md` 里已有的 leading words。ASD-STE100 Simplified Technical English 设定语域。Ubiquitous language 提供名词。Skill、`CLAUDE.md` 和 `CONTEXT.md` 伸手拿相同的 [tokens](https://www.aihero.dev/ai-coding-dictionary/token)，所以调用它不是新指令。它是 agent 已经同意的一条指令的提醒。

如果你没有 `CONTEXT.md`，skill 仍然有效。你只是失去 domain-vocabulary 那一半。

## It's working if

- Re-pitch **更短更清楚**，而不是更短更生硬。
- 它补上你缺失的前提，而不是只删词。
- 项目名词替换掉编造的。你 `CONTEXT.md` 里的 terms 回来了。
- 你能连续用两次，而它不会退化成一语带过。

## Where it fits

你可以在任何时间点、任何对话中、任何其他 skill 内部使用 `wait-what`。它事后修复一条消息。真正的 cure 是提前达成共识的 shared language，那是 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)：一场运行 [domain-modeling](https://aihero.dev/skills-domain-modeling) 的 [grilling](https://www.aihero.dev/ai-coding-dictionary/grilling) session，让你们双方使用的词都落进你的 `CONTEXT.md`。如果你不确定哪个 skill 适合当下，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
