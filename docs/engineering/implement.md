Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=implement
```

```bash
npx skills update implement
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/implement)

## What it does

`implement` 把 spec 或一组 issue 描述的工作落成代码：通过 test-driven development、typechecking 和完整测试套件推进，然后交给 review 并提交到当前 branch。

它**不**决定要构建什么。Spec 已经定稿，seams 也已经认可；`implement` 执行这个计划，而不是重新打开它。它是双手，不是大脑——思考在上游就已经发生了。

## When to reach for it

你通过输入 `/implement` 调用它——agent 不会自行触发。

当工作已经写成 spec 或拆成 issue，并且你准备把它转成代码时使用它。如果 spec 还不存在，先写出来——为此用 [to-spec](https://aihero.dev/skills-to-spec)，或用 [to-tickets](https://aihero.dev/skills-to-tickets) 把 spec 拆成 issue。如果你只想在没有完整 spec 的情况下 test-first 地构建某个东西，直接用 [tdd](https://aihero.dev/skills-tdd)。

## Pre-agreed seams

`implement` 赖以运行的概念是 **seam**——feature 被测试的稳定 interface，在写任何代码之前就选定。它不在 build 中途发明 seams；它使用已经选定的那些（在 [to-spec](https://aihero.dev/skills-to-spec) 期间），并通过 [tdd](https://aihero.dev/skills-tdd) 针对它们写测试。在预先认可的 seams 上工作，正是让实现保持诚实的原因：测试瞄准的是持久的东西，因此底下的代码可以变动，而测试不必跟着变动。

围绕这个核心，它让循环保持紧凑——频繁 typecheck、边走边运行单个测试文件、最后把整个套件跑一遍——然后以一次 review pass 和一次对当前 branch 的提交收尾。

## Where it fits

`implement` 是 main chain 末段、紧接在 review 之前的 build step：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

在工作已经写好 spec 并排好序之后使用它，而不是之前。它的关键邻居是 [to-tickets](https://aihero.dev/skills-to-tickets)——产出它所处理的 issue（每个 issue 都声明自己的 blocking edges），以及 [tdd](https://aihero.dev/skills-tdd)——它在内部驱动 tdd，在每个 seam 上先写好测试，然后再运行自己的 [code-review](https://aihero.dev/skills-code-review) pass 并提交。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
