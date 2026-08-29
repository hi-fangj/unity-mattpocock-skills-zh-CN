Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=prototype
```

```bash
npx skills update prototype
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/prototype)

## What it does

`prototype` 构建一个小型、一次性的程序，它唯一的职责是回答一个设计问题：这个 state model 感觉对吗，或者这个 UI 应该长什么样。

这份代码**从第一天起就是 throwaway 的**，并且被如此标记。它不带测试、除了能让它跑起来之外没有错误处理、没有抽象、也没有持久化。重点是快速学到点东西然后删掉它，所以一旦你开始加固它，你就不再是在做 prototype 了。

## When to reach for it

输入 `/prototype`，或者当任务合适时由 agent 自动触发。

当你有一个难以在纸面上定夺的设计问题时使用它：一个你无法在脑中同时握住所有 case 的 state machine，或者一个不看到几个版本并排就想象不出来的界面。如果相反，某个已经构建好的东西行为异常、你需要查明原因，那就用 [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs)；prototyping 探索的是该构建什么，而不是为什么已构建的东西坏了。

## Two branches

问题决定形状，而形状有两种：

- **"这个 logic / state model 感觉对吗？"**：一个自包含的单文件 HTML demo，任何人都能通过点击按钮驱动 state model。Free-play buttons 加 tabbed guided walkthroughs，把它交给非开发者（designer、PM、domain expert），让他们亲自感受这个 model。
- **"这应该长什么样？"**：在同一路由上的几个截然不同的 UI variations，可通过一个浮动栏切换，这样你对比的是真实渲染，而不是凭空想象。

选错分支会浪费掉整个 prototype，所以问题要先于一切。两个分支都把 state 保存在内存中、用一条命令运行，并在每一步都呈现完整 state。

## Keep the prototype as a primary source

一个完成的 prototype 会留下两样东西。**答案**：结论加上它所定夺的那个问题，是你要持久捕获的东西（一条 commit message、一份 ADR、一个 issue）。**prototype 本身则是一份 primary source**：答案赖以得出的可运行证据。

prototype 不属于 main branch：没有测试、没有错误处理、没有任何需要维护的东西。但这不是销毁它的理由。一旦答案被捕获，就把任何经过验证的决策折叠进真实代码，然后把 prototype 捕获到一条 throwaway branch 上（移出 main、永不合并），并在实现 issue 上留下一个指向它的 context pointer。main branch 保持干净；而那份原始探索对任何想重新运行它的人来说仍一键可达。一条烂在 main branch 里的 prototype 已经过了它的用途；一份作为 primary source 捕获在旁支 branch 上的 prototype 则没有。

## Where it fits

`prototype` 是一个可随时调用的 standalone：你进入它来解决一个设计问题，然后再退出来。它的答案往往会喂给下一步：一个经过验证的 state model 或 UI 方向会成为 [to-spec](https://aihero.dev/skills-to-spec) 要落笔写下的已定输入，或者一个值得通过 [domain-modeling](https://aihero.dev/skills-domain-modeling) 记录下来的架构决策。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
