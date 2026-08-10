Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=tdd
```

```bash
npx skills update tdd
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/tdd)

## What it does

`tdd` 用 test-first 的方式构建 feature 或修 bug：一次一个 behavior，通过 red-green loop 把代码驱动出来。

它**不会**先写完所有 tests。先批量写 tests（“horizontal slicing”）产出的是对_想象中的_ behavior 的 tests——它们检查事物的形状，却对真实的变化变得麻木。`tdd` 改取 vertical slices：一个 test，然后刚好足够通过它的代码，再下一个 test，每一个 cycle 都基于上一个 cycle 教会你的东西。Tests 只针对 public interfaces，因此底下的 implementation 可以变化，而 tests 不必跟着移动。

## When to reach for it

输入 `/tdd`，或者当任务合适时由 agent 自动触发——test-first 地构建 feature 或修 bug，或者当你说 “red-green-refactor” 时。

当有具体 behavior 要构建、并且希望 tests 能承受 refactor 时使用它。如果 behavior 还没钉住，先把 spec 定下来——为此使用 [to-spec](https://aihero.dev/skills-to-spec)。当工作真正关乎 interface 的形状而非 tests 时，使用 [codebase-design](https://aihero.dev/skills-codebase-design)；`tdd` 在 planning 期间会调用它来获取 deep-module 的词汇。

## Red-green, one slice at a time

主导思想是 **red-green loop**：写一个 failing test（red），加上刚好足够通过它的代码（green），然后为下一个 behavior 重复——每一个 cycle 都基于上一个 cycle 教会你的东西。第一个 cycle 是一颗 **tracer bullet**：一个证明单条路径 end-to-end 可用的 test，然后你才从它向外构建。因为代码是你刚写的，你确切地知道哪个 behavior 重要、如何验证它——你永远不会因为承诺了一个自己尚未理解的 test 结构而冲出车灯照亮的范围。

有两条规则让 tests 保持诚实。一个好的 test 读起来像一份 specification（“user can checkout with valid cart”），并通过 public API 演练真实的代码路径，因此重命名一个内部函数永远不会破坏它。而 expected values 来自一个独立的真相来源——一个已知正确的 literal、一个 worked example、spec——绝不按代码计算它们的方式重新计算，那正是一个 **tautological** test 按构造就通过、却什么也没告诉你的方式。

Refactoring 只在 suite 为 green 时进行；绝不在 red 时进行。

## It's working if

- 它写一个 test，让它通过，然后才写下一个——而不是一批 tests 后跟一批代码。
- Tests 命名 behaviors，而不是 internals，并且能在一次内部重命名后存活下来。
- Expected values 是来自 spec 的 literals，而不是按代码推导它们的方式推导出的数字。

## Where it fits

`tdd` 是 main build chain 用来写代码的 red-green loop：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

[implement](https://aihero.dev/skills-implement) 是这条 chain 的 build 步骤，它在内部驱动 `tdd`，以 test-first 的方式构建每个 ticket，然后交接给 [code-review](https://aihero.dev/skills-code-review)——所以 `tdd` 是该步骤内部的引擎，而不是一个独立的步骤。只要有具体 behavior 要构建、又没有完整 spec，你也可以直接调用它。它的另一个邻居是 [codebase-design](https://aihero.dev/skills-codebase-design)，`tdd` 依靠它来找到值得为之写 test 的 deep-module seams。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你引路。
