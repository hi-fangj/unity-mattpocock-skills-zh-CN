---
name: grilling
description: 围绕计划、decision 或 idea 持续追问用户。适用于用户想对自己的思路做压力测试，或使用任何 “grill” 触发措辞时。
---

持续访谈用户，直到达成 shared understanding。把它建模为 **design tree**：每个 decision 都会分支成挂在它下面的 decisions。

按 **rounds** 推进这棵树。**frontier** 是 prerequisites 已确定的所有 decisions：你现在就能问、而不必猜测还没听到的答案的问题。一轮内问完整个 frontier：给每个问题编号，并给出你的推荐答案。然后等待用户的回答，再进行下一轮。

一轮的格式如下：

```
❓ **Q1** - **<question title>**：<question body，可以是多段，包括多个选项>

➡️ <你的推荐答案>

---

❓ **Q2** - **<question title>**：<question body，可以是多段，包括多个选项>

➡️ <你的推荐答案>
```

每轮用户的回答都会重塑这棵树：已确定的 decisions 把 frontier 向外推，并解锁依赖它们的问题。重新计算 frontier，再问下一轮。一个答案依赖本轮仍开放的另一问题的 question，属于 _后面_ 的轮次，不属于本轮。

查找 _facts_ 是你的工作，绝不是用户的。当 frontier 问题需要来自 environment（filesystem、tools 等）的 fact 时，派一个 sub-agent 去查；不要向用户索要任何你自己能查到的内容。不要为此阻塞：进行中的探索是一个未确定的 prerequisite，所以只有它下游的问题才等待 sub-agent 汇报，现在就把 frontier 的其余部分问完。_decisions_ 属于用户：逐个交给他，并等待。

当 frontier 为空时 session 结束：design tree 的每个分支都已走完，没有任何东西被默默假设。在用户确认你们已达成 shared understanding 之前，不要采取行动。
