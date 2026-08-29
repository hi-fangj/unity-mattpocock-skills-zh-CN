Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=research
```

```bash
npx skills update research
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/research)

## What it does

`research` 通过阅读拥有答案的 sources 来回答一个问题，并留下一份带引用的 Markdown 文件。它只基于 **primary sources** 工作（official docs、source code、specs、first-party APIs），绝不使用对它们的二手转述，因此它保存下来的东西可以追溯到某个权威来源，而不是对一份总结的再总结。

## When to reach for it

输入 `/research`，或者当任务变成阅读类 legwork 时由 agent 自动触发。

当下一步是*弄清楚某件事*时使用它（一个 API 如何表现、一份 spec 实际说了什么、某个说法是否成立），而你不想为了做这些阅读而卡住自己的 thread。要通过访谈而非阅读来打磨计划，用 [grilling](https://aihero.dev/skills-grilling)；要用 throwaway code 探索该构建什么，用 [prototype](https://aihero.dev/skills-prototype)。

## Delegated legwork

它的标志性动作是：阅读以 **background agent** 的形式运行。你继续工作；它离开去把每条说法追溯回其 primary source，然后把一份带引用的 Markdown 文件放进 repo 存放此类笔记的地方。Research 是你委派出去的 legwork，而不是你外包出去的思考：你拿回的是一份可供你回应的文档，附带它的来源。

## Where it fits

一个可随时调用的 standalone，为思考类 skills 供料：它产出的文件是用来 grill、plan 或 design 的对象，因此它位于 [grilling](https://aihero.dev/skills-grilling) 和 [to-spec](https://aihero.dev/skills-to-spec) 这类工作的上游，而不在 build chain 之中。完整地图见 [ask-matt](https://aihero.dev/skills-ask-matt)。
