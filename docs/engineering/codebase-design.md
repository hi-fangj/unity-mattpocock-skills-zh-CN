Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=codebase-design
```

```bash
npx skills update codebase-design
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/codebase-design)

## What it does

`codebase-design` 为你提供一套共享、精确的词汇，用于设计 **deep modules**：大量行为藏在一个小 interface 后面，放置在一条干净的 seam 上，并且可以通过那个 interface 来测试。

它是一种**语言，而不是一套流程**。它不会重构你的代码，也不会递给你一份 refactor 计划：它修正的是那些词（module、interface、depth、seam、adapter、leverage、locality），让每一次设计对话以及每一个涉及设计的 skill 都用同样的方式说话。语言的一致性是全部要点；"component"、"service"、"API" 和 "boundary" 被刻意禁用，因为它们模糊了那些重要的区分。

## When to reach for it

输入 `/codebase-design`，或者当任务契合时由 agent 自动调用。

当你在设计或改进某个 module 的 interface、寻找 deepening 的机会、决定 seam 该放在哪里，或者让代码更易测试、更易被 AI 导航时，使用它。其他 skills 在需要 deep-module 词汇时也会引入它。如果你想打磨的是项目的*领域*术语而非 module 设计，请改用 [domain-modeling](https://aihero.dev/skills-domain-modeling)；要对现有 codebase 做一整轮架构梳理，用 [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture)。

## Deep, not shallow

当一个 module 有大量行为藏在一个小 interface 后面时，它是 **deep** 的；当 interface 几乎和实现一样复杂时，它是 **shallow** 的。深度以 **leverage** 来衡量：调用方（或一个 test）每学习一单位 interface 就能驱动多少行为。关键在于，深度是 *interface* 的属性，而不是实现的属性：一个 deep module 内部可以由小的、可替换的部件组成，只是这些部件从不暴露给调用方。

有两个检查承担了大部分工作。**deletion test**：想象删掉这个 module，如果复杂性消失了，那它只是一个 pass-through；如果复杂性在 N 个调用方中重新出现，那它就是在挣它的口粮。以及 **一个 adapter 意味着一条假设性的 seam；两个 adapters 意味着一条真实的 seam**：在真正有东西跨越它而变化之前，不要切开 seam。

## The interface is the test surface

调用方和 tests 跨越同一条 seam，所以一个放置得当的 interface 给了 tests 一个持久的瞄准点，而底下的代码可以自由移动。这就是为什么这套词汇坚持使用 **seam**（Feathers 的术语：一个无需在那里编辑就能改变行为的地方），而不是被过度使用的 "boundary"；也是为什么这里的 "interface" 指的是*调用方必须知道的每一个事实*：签名，是的，但也包括不变量、顺序、错误模式和性能，而不仅仅是类型层面的表面。

## Pulled out on purpose

`codebase-design` 是 deep-module 词汇的**唯一权威来源**，它被拆出来作为自己的 model-invoked skill，这样任何东西都能触达它。其他 skills 指向它，而不是重述那些词：[tdd](https://aihero.dev/skills-tdd) 借用它在写 test 之前放置 seam，[improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) 在重构现有代码时依赖它，而 [to-spec](https://aihero.dev/skills-to-spec) 在写 spec 之前勾勒 seams 和 deepening 机会时会说这套语言。

保持它 standalone 的意义在于，你也可以单独调用它，作为关于如何思考 module 设计的**参考**，而不触发那些 skills 所要求的更大流程。在一个地方把词修正一次，每一次设计对话都会继承它们。

## Where it fits

`codebase-design` 是一个**随时可调用的 standalone**：engineering skills 之下的共享词汇层。它最接近的邻居是 [domain-modeling](https://aihero.dev/skills-domain-modeling)，那是针对问题领域而非 module 结构的平行词汇 skill。当你不确定哪个 skill 或 flow 契合时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
