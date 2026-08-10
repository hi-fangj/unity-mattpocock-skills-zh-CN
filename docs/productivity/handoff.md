Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=handoff
```

```bash
npx skills update handoff
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/handoff)

## What it does

`handoff` 把当前对话压缩成一份 **handoff document**——一篇完整的交接说明，一个全新的 agent 读完它就能从你停下的地方接着干。

它**不会**复述已经存在于别处的内容。凡是已经记录在 spec、plan、ADR、issue、commit 或 diff 中的东西，都只通过路径或 URL 引用，绝不复制。这份文档只承载 live thread——你在做什么、为什么、下一步是什么——并且它被保存到你操作系统的临时目录里，而不是 workspace 中，因此它永远不会变成又一份需要维护的 artifact。

## When to reach for it

你通过输入 `/handoff` 来调用它——agent 不会自行调用。附上一句关于下一个 session 用途的说明，文档就会据此量身裁剪。

当一场对话已经长到它的 context 岌岌可危时——你快要触及 context 上限、准备收工、或者有意把工作交给另一个 agent——就用它，好让这条 thread 被保存下来，而不必拖着整份 transcript 一起走。

## What the document carries

- **Live thread**——用对话自己的措辞，说明什么事情正在进行中、为什么，并减去任何已经在别处写下来的内容。
- **Suggested skills**——一个指针，指向下一个 agent 应该调用哪些 skills 来继续。
- **引用，而非复制**——指向那些承载已定细节的 specs、plans、ADR、issues 和 diffs 的链接与路径。
- **Redacted secrets**——API keys、密码和 PII 在文档写出之前就被剥离。

要抓住的核心概念是 **compaction**：一次 handoff 就是把对话挤压到只剩它可恢复的内核，好让一个全新的 agent 继承的是势头，而不是噪音。

## Where it fits

`handoff` 是一个随时可调用的 standalone——它位于两个 sessions 之间的接缝处，而不是某条 build chain 内部。它与那些产出 artifact 的 skills 天然搭配，因为它指向的正是它们的产出：[to-spec](https://aihero.dev/skills-to-spec)，因为一份完成的 spec 恰恰是那种 handoff 会引用而非重复的已定细节。当你不确定哪个 skill 适合当下时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
