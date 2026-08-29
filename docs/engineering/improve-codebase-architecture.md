Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=improve-codebase-architecture
```

```bash
npx skills update improve-codebase-architecture
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/improve-codebase-architecture)

## What it does

`improve-codebase-architecture` 扫描 codebase，寻找 **deepening opportunities**：那些 shallow module（interface 几乎和它所隐藏的东西一样复杂）可以变深的地方，把它们呈现为一份自包含的可视化 HTML report，然后就你选中的那一个继续 grilling。

它**不是**递给你一份扁平的 refactor 清单。每个候选项都必须通过 **deletion test**：移除这个 module 会把复杂性*集中*到一个更小的 interface 后面，还是只是把它挪来挪去？只有"会集中"的情况才配得上一张卡片。正是这道过滤，让 report 不至于沦为泛泛的清理建议。

除非你把它指向某个特定区域，否则它还会把自己限定在开发实际落地的地方：读取最近的 commits，偏向于你仍在修改的代码。Deepening 一个 module 的收益在于让它未来的修改更容易，因此它会对 repo 中最近仍在变化的部分额外加权。

## When to reach for it

你通过输入 `/improve-codebase-architecture` 调用它，agent 不会自行触发。

把它当作定期的健康检查来用：每隔几天，或者每当 codebase 开始让人觉得要理解一个概念就得在大量小 module 之间来回跳转时。它读取现有架构，并提出可以在哪里加深它。如果你已经知道想重新设计哪个 module，只是需要一套词汇来把它想清楚，那就改用 [codebase-design](https://aihero.dev/skills-codebase-design)：这个 skill 是找出候选项的勘察，那个才是设计工作台。

## Deepening opportunities

整个 skill 围绕一个概念运转：**depth**。一个 deep module 把大量功能藏在一个小而稳定的 interface 后面；一个 shallow module 则会透过一个几乎和底下代码一样宽的 interface 泄漏自己的实现。这份 report 搜寻的是浅薄：纯粹为了可测试性而抽出的 pure function（真正的 bug 藏在它们被调用的方式里，没有 **locality**）、跨 **seam** 泄漏的 module、不打开五个文件就无法理解的概念，并提出能修复它的 deepening。

它用共享的设计词汇（**module**、**interface**、**depth**、**seam**、**adapter**、**leverage**、**locality**）以及你项目自己来自 `CONTEXT.md` 的领域语言来表达，因此一个候选项读起来是"加深 Order intake module"，而绝不是"重构 FooBarHandler"。

## The report, then the grill

输出是一份可直接在浏览器打开的 HTML 文件，写入你操作系统的临时目录，不会有任何东西落进 repo。每个候选项都是一张卡片，包含涉及的文件、摩擦点、一段平实的解决方案、以 locality 和 leverage 衡量的收益、一张 before/after 示意图，以及一个 `Strong` / `Worth exploring` / `Speculative` 徽章。它最后会给出自己最想先处理的那一个。

然后它停下来，问你想探索哪一个。选中一个，它就会针对那个设计运行 [grilling](https://aihero.dev/skills-grilling) 循环（约束、seam 后面放着什么、哪些测试能存活），并在决策逐渐清晰时就地更新 domain model。

## Where it fits

`improve-codebase-architecture` 是**定期维护**：每隔几天运行一次，而不是作为链条中的一步。它的邻居是 [codebase-design](https://aihero.dev/skills-codebase-design)（它拥有每个候选项赖以书写的 depth-and-seam 词汇），[grilling](https://aihero.dev/skills-grilling)（一旦你选中候选项就由它走 decision tree），以及 [domain-modeling](https://aihero.dev/skills-domain-modeling)（在重新设计落定时保持 `CONTEXT.md` 和 ADR 处于最新状态）。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
