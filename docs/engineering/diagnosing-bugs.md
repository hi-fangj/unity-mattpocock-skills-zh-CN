Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=diagnosing-bugs
```

```bash
npx skills update diagnosing-bugs
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/diagnosing-bugs)

## What it does

`diagnosing-bugs` 为棘手 bug 和性能回退运行一套纪律化的诊断循环：构建一个 repro，把它最小化，对 hypotheses 排序，instrument，然后带着 regression test 修复。

它拒绝在你拥有 **tight feedback loop** 之前就提出假设：那是一条已经能在*这个* bug 上变红的可运行命令。在那条命令存在之前读代码来构建理论，正是本 skill 要防止的那个失败。没有能变红的 loop，就没有诊断。

## When to reach for it

输入 `/diagnosing-bugs`，或者当任务契合时由 agent 自动调用：它会在 "diagnose" / "debug this" 时触发，或者当你报告某些东西 broken、throwing、failing 或 slow 时触发。

在那些难啃的问题上使用它：一眼看不出的 bug、间歇性的 flake、在两个 known-good states 之间悄悄潜入的 regression。如果只是想快速做一个用完即弃的东西来检验一个设计问题，而不是追查一个缺陷，请改用 [prototype](https://aihero.dev/skills-prototype)。

## The tight loop is the skill

其他一切（bisection、hypothesis-testing、instrumentation）一旦你有了信号就都是机械操作。所以本 skill 在 Phase 1 上投入了不成比例的精力：构建一个 pass/fail 命令，它驱动真正的 bug 代码路径并断言用户的确切症状，然后**收紧**它，直到它快速、确定、并且可由 agent 运行。一个 30 秒的 flaky loop 几乎和没有一样糟；一个 2 秒的确定性 loop 则是调试超能力。

它给你一架梯子来构建那个 loop（failing test、curl script、CLI diff、headless browser、replayed trace、throwaway harness、fuzz loop、`git bisect run`、differential run），并且，只有作为最后手段，才用一个 human-in-the-loop 的 bash script。对于非确定性 bug，目标不是一个干净的 repro，而是一个**更高的复现率**：循环触发、并行化、施加压力，直到 flake 变得可调试。

## It's working if

- 它在提出理论*之前*就构建并运行一个 repro 命令，并粘贴出调用方式及其变红的输出。
- 这个 loop 断言的是你实际报告的症状，而不是一个相邻的失败。
- Hypotheses 以一个排好序的、可证伪的清单呈现给你，在任何一条被测试之前。
- Debug instrumentation 被打上标签（`[DEBUG-...]`），并在它宣布完成之前被 grep 清除掉。

## Where it fits

`diagnosing-bugs` 是一个随时可调用的 standalone：你在某些东西坏掉的那一刻进入它，并在修复及其 regression test 就位后退出。它的 post-mortem 会在真正的发现是没有一条好的 seam 来锁住这个 bug 时，移交给 [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture)，问题在于代码，而不在于 bug。当你不确定哪个 skill 契合时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
