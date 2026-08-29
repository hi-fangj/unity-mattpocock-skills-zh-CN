# Skill mechanics

[`writing-for-agents`](SKILL.md) 的 skill-specific 分支：当文档是 skill 时什么会变，即 frontmatter、invocation 选择和 router skills。其他关于写作的内容，都是 `SKILL.md` 里的 universal reference。

## Invocation

两个选择，在两个 loads 之间做交易：

- **Model-invoked** skill 保留 `description`，所以 agent 能自主触发它，而且其他 skills 也能到达它。你仍然可以输入它的名字：model-invocation 总是_包含_ user reach；description 只增加 agent discovery，绝不拿走 human 的。Description 是 skill 的顶层 context pointer，被迫时刻保持加载，用永久的 context load 换 discoverability。内容全是 reference 的 model-invoked skill 也是 shared reference 的一个家：另一个 skill 可以调用它，所以几个 skills 需要的 reference 住在同一个地方。Mechanics：省略 `disable-model-invocation`，并写一个携带 trigger branches 的 model-facing description（`SKILL.md` 中的 pointer-writing rules 完整适用）。
- **User-invoked** skill 把 description 从 agent 的 reach 中剥掉：只有人类输入它的名字才能调用它，其他 skill 都不能。零 context load，但它花 cognitive load：你是必须记得它存在的索引。Mechanics：设置 `disable-model-invocation: true`；`description` 变成 human-facing：一行 summary，去掉 trigger lists。

只有 agent 必须自己到达这个 skill、或另一个 skill 必须到达它时，才选 model-invocation。如果它只靠人手触发，就做成 user-invoked，不付 context load。

两个 user-invoked skills 都需要的 shared reference 不能住在任何一边：没有 descriptions，谁都无法触发另一个。把它推到 skill system 之外的 plain file：任何 skill 都能指向的 external reference。

## Splitting by invocation

Splitting 的 invocation cut（sequence cut 在 `SKILL.md` 里）：当你有一个应当独立触发它的 distinct leading word（你在 prompts 里真正使用的 trigger word）或另一个 skill 必须到达它时，拆出一个 model-invoked skill。你为新的 always-loaded description 付出 context load，所以那份独立 reach 必须值得。

## Router skills

当 user-invoked skills 多到超出你记得住的量，堆积的 cognitive load 由 **router skill** 治愈：一个 user-invoked skill，说出其他 skills 以及何时去拿每一个，这样 human 只需要记住一个 skill 而不是许多。它只能 hint，永远不能触发它们：user-invoked skills 没有 description，所以除了 human 没有任何东西能到达它们。
