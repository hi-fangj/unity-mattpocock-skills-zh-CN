Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=ask-matt
```

```bash
npx skills update ask-matt
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/ask-matt)

## What it does

`ask-matt` 是本仓库 skills 的 router。你描述当前情境，它告诉你应该走哪个 skill 或 flow，以及按什么顺序运行。

它**本身不执行任何工作**。它不 grill、不写 spec、也不修任何东西，它只负责定位。它尤其为 **user-invoked** skills 而存在：没有任何东西会替你触发那些 skills，所以*你*必须记得它们存在，而 `ask-matt` 就是你把这份记忆外包出去的地方。它也指向那些你会按名字调用的 model-invoked skills：`/unity-development`、`/tdd`、`/diagnosing-bugs`、`/prototype`、`/code-review`，以及两个 vocabulary 参考，`/domain-modeling` 和 `/codebase-design`。它回答「哪一个、什么时候用」，然后把你交给真正干活的 skill。

## When to reach for it

你通过输入 `/ask-matt` 调用它，agent 不会自行使用它。

当你不确定某个情境该用哪个 skill 或 flow 时使用它：你有一个 idea 但不知道从哪里开始，有一堆 bug reports 但不确定是否该交给 `/triage`，或者两个 skills 看起来可以互换而你分不清它们。如果你已经知道要用哪个 skill，就跳过 router，直接调用那个 skill。

## Flows, not just skills

`ask-matt` 给你用来思考的核心概念是 **flow**：一条穿*过*多个 skills 的路径，而不是单个 skill。大多数工作沿着一条 **main flow** 进行（idea → ship：grill → spec → tickets → implement → review），有两条 **on-ramp** 汇入其中（一条处理传入 bugs 和 requests 的 triage lane；一条生成 ideas 的 codebase-health lane），其余一切都是可以单独调用的 **standalone**。提出一个问题，你就会被放到正确的 flow 上正确的步骤，而不只是被递上一个工具。

## Where it fits

`ask-matt` 是 **router**：悬于整套 skills 之上的 standalone 地图。它是每个其他 docs 页面都回链到的节点（[ask-matt](https://aihero.dev/skills-ask-matt)），所以它从不*位于*某条 chain 之中；它*指向*每一条 chain。从这里你最常落到 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)（main flow 的头部），或 [triage](https://aihero.dev/skills-triage)（那些不是你创建的工作的 on-ramp）。当连 router 自己的图景都过期时，它的 [Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/ask-matt) 才是权威地图。

在 Unity repository 中，router 会把 [unity-development](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/unity-development) 作为 execution reference，避免默认套用通用 `/implement`、`/tdd`、browser prototype 或 compiler-only verification 假设。
