Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=grilling
```

```bash
npx skills update grilling
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/grilling)

## What it does

`grilling` 是一场不依不饶的访谈，在你动手构建之前对计划或设计做 stress-test。它沿着 decision tree 一个分支一个分支地走下去，逐一解决 decisions 之间的依赖关系，直到你和 agent 达成共同理解。

它**一次只问一个问题**，并等待你的回答后再问下一个，绝不会抛出一大堆问题清单，那会让人不知所措。每个问题都附带 agent 自己推荐的答案，而任何 codebase 能够回答的问题，它会自己去探索而不是问你。在你确认已达成共同理解之前，它不会开始执行计划。

为避免小 feature 被无休止地拷问，`grilling` 使用 **30+20** 检查点：完成 30 个问题后，检查业务目标、使用场景、流程、规则、异常、边界和矛盾描述；之后每 20 个问题重复一次。若没有会影响实现的核心缺口，就停止提问并输出需求确认文档；已经被前置规则排除、不会改变实现的理论边界不会被重复追问。

## When to reach for it

输入 `/grilling` 即可，或者当任务合适时 agent 会自动调用它，这是底层的 primitive，而不是仅供用户使用的入口。

当计划或设计仍有软肋、你想在写代码之前把它们暴露出来时，就用它。实践中你通常通过它的两个 wrapper 之一来调用，而不是直接点名：纯 grilling session 用 [grill-me](https://aihero.dev/skills-grill-me)；想让 session 在过程中同时写出 ADR 和 glossary，用 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)。

## The decision tree

心智模型是一棵 **decision tree**：每个计划都会分支成若干 decisions，而 decisions 之间相互依赖。`grilling` 一次下降一个节点，因此早期的答案能够重塑接下来会出现哪些问题。这正是问题逐个出现、并按依赖顺序排列的原因：并行问题的“消防水龙”会丢掉让访谈收敛到共同理解的那个结构。

## Pulled out on purpose

`grilling` 是这套访谈技术的 **single source of truth**，被拆出来作为一个 model-invoked **primitive**，让每一个需要访谈的 skill 都能调用它，而不必各自重新发明一套。[grill-me](https://aihero.dev/skills-grill-me) 和 [grill-with-docs](https://aihero.dev/skills-grill-with-docs) 是它的两个 user-invoked 前门，但 [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) 和 [triage](https://aihero.dev/skills-triage) 也依赖它来 pressure-test 它们自己的 decisions。

把这套技术放在一处，也意味着当你只想要访谈本身时可以直接调用它，而不必带上它的 wrappers 附加的 ADR 写作或 ticket shaping。

## Where it fits

`grilling` 是主 build chain 之下的访谈 **primitive**：[grill-with-docs](https://aihero.dev/skills-grill-with-docs) 运行它来打磨 context，然后才由 [to-spec](https://aihero.dev/skills-to-spec) 写出 spec。当你不确定哪个入口合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
