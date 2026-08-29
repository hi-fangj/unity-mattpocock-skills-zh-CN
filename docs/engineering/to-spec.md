Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=to-spec
```

```bash
npx skills update to-spec
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/to-spec)

## What it does

`to-spec` 把当前 conversation 和你对 codebase 的理解整理成一份 spec（你可能把这份文档称为 PRD），然后发布到你的 issue tracker。

它**不会**再次访谈你。当你调用它时，alignment 工作已经完成：`to-spec` 综合的是已经知道的内容，而不是再问一轮问题。

## When to reach for it

你通过输入 `/to-spec` 调用它，agent 不会自行触发。

当一个 change 已经充分讨论、domain language 已经确定，并且你希望在写任何代码之前把这份共同理解写下来时，就使用它。如果你*还*没有对齐，先 grill，为此使用 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)。要把完成的 spec 拆成 tickets，使用 [to-tickets](https://aihero.dev/skills-to-tickets)。

## Prerequisites

`to-spec` 会发布到你的 issue tracker，因此 [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) 必须先为这个 repo 配置好 tracker 和 triage labels。它自行应用 `ready-for-agent` label，不需要单独的 triage 环节。

## What the spec includes

- **Problem statement**：用项目自己的词汇说明什么坏了或缺失了，以及为什么值得解决。
- **Solution**：高层的修复形状，在任何 implementation detail 之前。
- **User stories**：一份详尽、编号的具体 behaviors 列表，即这个 change 必须支持的内容，每一条都可独立检查。
- **Implementation decisions**：对话期间已经确定的选择，这样它们之后不会被重新争论。
- **Testing decisions**：feature 将在哪些 seams 处被测试，以及 “done” 是什么样子。
- **Out-of-scope items**：这个 change 刻意*不*覆盖的内容，以让 ticket 保持有界。
- **Further notes**：其他值得延续、但不适合上面各小节的内容。

## Deep modules

在写 spec 之前，`to-spec` 会勾勒 feature 将被测试的 **seams**，并寻找 **deep module** 的机会：大量功能藏在一个小而稳定的 interface 之后。它优先使用 existing seams 而非新建，并尽可能使用最高的 seam，理想情况下整个 change 只有一个。

这对 agentic 开发很重要：一个好的 interface 给 tests 一个持久的 target，因此底下的代码可以变化，而 tests 不必跟着移动。

## It's working if

- 它开始写 spec，而不是再问你一轮问题。
- 它在写之前与你核对 seams，并提议尽可能少的 seams。
- Spec 以你项目的 domain vocabulary 返回，而不是通用样板。

## Where it fits

`to-spec` 是 main build chain 中的一个步骤：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

在 plan 和 domain language 解决之后、在把工作拆成 implementation tickets 之前使用它。它的关键邻居是 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)（它打磨 context 使 spec 精确），以及 [to-tickets](https://aihero.dev/skills-to-tickets)（它把 spec 变成一组供 [implement](https://aihero.dev/skills-implement) 构建的 tickets）。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你引路。
