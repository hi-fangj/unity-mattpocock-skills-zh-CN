Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=to-tickets
```

```bash
npx skills update to-tickets
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/to-tickets)

## What it does

`to-tickets` 把一个 plan、spec 或当前 conversation 拆成一组 **tickets**——每个都是一颗 tracer-bullet vertical slice——并发布到你配置好的 tracker，其中每个 ticket 都声明了 block 它的 tickets。

每个 ticket 都是一颗 **tracer bullet**——一条贯穿所有集成层、end-to-end 的窄*垂直*切片（schema、API、UI、tests），绝不是某一层的 horizontal slice。一个完成的 slice 可以独立 demo 或 verify，这正是让每个 ticket 可以安全交给 agent 的原因。

## When to reach for it

你通过输入 `/to-tickets` 调用它——agent 不会自行触发。

当你已经有了一份 agreed plan 或 written spec、并希望把它拆成 tickets 时使用它。把它指向 conversation，或者传入一个 spec 或 issue reference，它会先抓取 body 和 comments。如果这个 change 还没有被写成 spec，先产出一份——为此使用 [to-spec](https://aihero.dev/skills-to-spec)。

## Prerequisites

`to-tickets` 会发布到你的 issue tracker，因此 [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) 必须先为这个 repo 配置好 tracker 及其 triage label vocabulary。在真实 tracker 上，它会在发布时应用 ready-for-agent label。

## One artifact, two readings

Blocking edges 才是重点所在。它们让同一组 tickets 有两种读法，取决于 tracker：

- **Local files** → `.scratch/<feature>/issues/` 下每个 ticket 一个文件，按 blockers-first 编号，edges 写成文本。你从上到下、手工处理它们，保持在 loop 之中。
- **真实 tracker（GitHub、Linear）** → 每个 ticket 一个 issue，edges 是 native blocking links（或 sub-issues）。任何 blockers 都已完成的 ticket 都位于 **frontier**，可以被领取——因此多个 agents 可以同时运行。

无论介质如何，edges 都存在于 ticket 之中；介质只决定是否有东西并行地作用于它们。`to-tickets` 产出 artifact——你如何运行它（手工顺序执行，还是一个并行的 fleet）由你决定。

## Vertical slices, not horizontal ones

整个 skill 取决于一个区别。**Horizontal** slice 交付 change 的一个层——全部 schema，或全部 API——在每一层都落地之前什么都不能工作。**Vertical** slice，即 tracer bullet，一次性交付穿过*每一*层的一条窄路径，因此它完成的瞬间就可以被 demo。

在切片之前，`to-tickets` 会寻找 prefactoring——“make the change easy, then make the easy change”——并把那部分工作排在最前。然后它会就拆分方案向你提问（粒度、blocking edges、什么该合并或拆分），再发布任何内容，并且 blockers-first 地发布，这样每个 ticket 的 “Blocked by” 才能引用一个真实存在的 ticket。

## The wide-refactor exception

有一种形状打破了 tracer-bullet 规则：**wide refactor**——一个单一的机械性变更（重命名一个 column、重新定义一个共享 symbol 的类型），其 **blast radius** 扇形展开到整个 codebase，因此一次编辑就会同时破坏数千个 call sites，没有任何 vertical slice 能以 green 落地。`to-tickets` 改以 **expand–contract** 来切分它：expand（在旧形式旁添加新形式，这样什么都不破坏）、migrate（按 blast radius 分批迁移 call sites，每批一个 ticket，因为旧形式仍然存在，所以 CI 全程保持 green），然后 contract（一旦没有调用者残留就删除旧形式）。当连 batches 都无法独自保持 green 时，它们共享一个 integration branch，全部 block 一个最终的 integrate-and-verify ticket，green 只在那里被承诺。

## Where it fits

`to-tickets` 是 main build chain 中的一个步骤：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

它位于 [to-spec](https://aihero.dev/skills-to-spec)（交给它一份已定稿、带 user stories 可供切片的 spec）和 [implement](https://aihero.dev/skills-implement)（构建每个 ticket，在内部驱动 [tdd](https://aihero.dev/skills-tdd) 以 test-first 的方式写 tests，然后进行 [code-review](https://aihero.dev/skills-code-review) 环节）之间。按 frontier 推进，每个 fresh context 一个 ticket，在它们之间清理。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你引路。
