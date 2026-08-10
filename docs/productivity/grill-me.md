Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=grill-me
```

```bash
npx skills update grill-me
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/grill-me)

## What it does

`grill-me` 围绕一个计划或设计进行一场不依不饶的访谈，走遍 decision tree 的每一个分支，直到你和 agent 达成**共同理解**。

它**一次只问一个问题**，然后等待。它绝不会一股脑把一批问题倒给你——那会让人不知所措——而当某个问题可以通过阅读 codebase 来回答时，它会自己去读而不是问你。每个问题都附带 agent 自己推荐的答案，所以你是在对一个提案做出反应，而不是盯着一个空白的提示发呆。

## When to reach for it

你通过输入 `/grill-me` 来调用它——agent 不会自行调用。

在构建之前，当一个计划大体上感觉对了、但你能感觉到其中藏着尚未解决的 decisions 时——也就是你想让软肋被找出来、被摊到台面上的那个时刻——就用它。如果你希望同样的追问还能留下一串 ADR 和 glossary 的书面痕迹，改用 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)。而如果这件事大到一次 session 装不下、通往目标的路径仍然模糊——一个 greenfield 项目、一次庞大的功能构建——那就从更上游的 [wayfinder](https://aihero.dev/skills-wayfinder) 开始，它先把事情规划成一张 decisions 的地图，然后再汇回这条流程。

## The decision tree

Session 把计划当作一棵 decisions 组成的树来走，逐一解决它们之间的依赖关系——先确定 parent decision，再处理挂在它下面的那些 choices。重点不是尽快达成一致，而是让每一个隐含的判断都变得明确，好让任何重要的事情都不会被默默地当成理所当然。走到另一头时，你会得到一份所有分支都已被走过的计划。

`grill-me` 是 **stateless** 的：它什么都不写，也不留下任何 workspace。它在任何地方都能运行，唯一的产物就是对话本身那份被打磨清晰的理解。这正是它与 [grill-with-docs](https://aihero.dev/skills-grill-with-docs) 的刻意对比——后者把同一场访谈捕获为持久的 ADR 和 glossary。

## Where it fits

`grill-me` 是一个随时可调用的 standalone——每当一个计划需要被强化时，你就运行它做构建前的 stress-test。它是 [grilling](https://aihero.dev/skills-grilling) primitive 的 stateless、user-invoked 前门；与它最邻近的是 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)，那个 stateful 的兄弟，运行同样的访谈，但额外把 decisions 记录为 ADR 和 glossary。如果结果是一份你想写下来的 spec，就移交给 [to-spec](https://aihero.dev/skills-to-spec)，它把已达成的理解综合成一份 spec，而无需重新访谈你。当你不确定哪条流程合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
