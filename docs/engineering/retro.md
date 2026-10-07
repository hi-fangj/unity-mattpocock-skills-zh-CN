Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=retro
```

```bash
npx skills update retro
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/retro)

## What it does

`retro` 回顾一次 coding [session](https://www.aihero.dev/ai-coding-dictionary/session)，对 agent 的**[环境](https://www.aihero.dev/ai-coding-dictionary/environment)**提出改进建议，让下一次运行更好。它读取 session 自身的记录（默认当前 session，也可以指向 session logs 中的某一次），找到 agent 挣扎过的时刻，按严重程度从高到低给你一份候选项清单。

它改的是环境，不是代码。Agent 发布的 bug、它用了二十次 [tool calls](https://www.aihero.dev/ai-coding-dictionary/tool-call) 才找到的文件、reviewer 漏掉的规则：`retro` 都不直接修，而是问 repo 中是什么让它们得以发生，并提出下次能阻止它们的 check、pointer 或 standard。它也只提议：你选中候选项之前，什么都不会改变。

## When to reach for it

你通过输入 `/retro` 调用它，agent 不会自行使用它。

在一次比预期艰难的 session 结束时使用它：agent 找某个东西找了太久、犯了一个工具本可抓住的错误、或需要一条它拿不到的信息。顺利的 session 没什么可教的，findings 来自困难的那些。想对 session 产出的代码下结论，用 [code-review](https://aihero.dev/skills-code-review)。

## Findings 落在哪里

每个候选项属于一个类别，类别决定修复的去处：找文件或事实太慢，用一个 agent 已经会读的文件里的 **navigation pointer**；工具本可抓住的错误，用一个 **automated check**（lint rule、type、test、pre-commit hook、CI job）；reviewer 漏掉 judgement-call 错误，给 reviewer agent 一条 `CODING_STANDARDS.md` 规则；`AGENTS.md`/`CLAUDE.md` 过大，把 steering 移进 standards 或 checks；昂贵的 tool call，精简或替换；不改变任何行为的行，删掉 **no-ops**；agent 需要但拿不到的信息，扩大访问（tee dev server log、只读访问服务）。

核心思想：standards 属于 **reviewer**，不属于 implementer。Implementation agent 的 [context](https://www.aihero.dev/ai-coding-dictionary/context) 压力最大（要探索、写代码、调试）；review agent 只拿 diff。所以新规则放在有空间执行它的地方，即 review，而不是无论是否相关都会加载进每个 session 的 [AGENTS.md](https://www.aihero.dev/ai-coding-dictionary/agents-md)。写规则前先分类违规：**mechanical** 违规（被禁 API、import 形状、文件位置规则）交给 deterministic check，因为 check 会失败而 standards 文件里的句子不会；只有真正的 judgement calls 才变成 prose。如果 repo 完全没有 guardrail（没有 pre-commit hook，也没有跑 lint、typecheck 和 tests 的 CI job），`retro` 会把这本身报告为一个 finding。

它只提议、不自动安装，这需要 judgement 决定什么值得一个永久 check；也只审计它读到的那一个 session，不会回顾自己上个月提出的规则，所以你仍要自己修剪 check：一条对好代码频繁触发的规则就是移除它的信号。每个候选项都必须能追溯到 session 记录中的具体时刻，无法追溯的丢弃。

## Where it fits

`retro` 是 main chain 的最后一步，用来看这条链走得如何：

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review → retro
```

在一次值得学习的构建之后运行它，在同一个 session 里，或指向该 session 的 log；顺利的构建可以跳过。session 已滑出 [smart zone](https://www.aihero.dev/ai-coding-dictionary/smart-zone) 时，先 [clear](https://www.aihero.dev/ai-coding-dictionary/clearing)，再让一个 fresh `/retro` 指向 session logs 中的上一次 session。

[code-review](https://aihero.dev/skills-code-review) 是 `retro` 最常调优的 reviewer agent，新的 coding standards 写到它的 Standards 轴线会读取的地方；[writing-for-agents](https://aihero.dev/skills-writing-for-agents) 为 `retro` 提议的每个 steering file 和 skill 设定写作风格，`retro` 启动时会先加载它；[improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) 只需要代码、寻找代码的结构改进，`retro` 需要 session 历史、改进 agent 所处的环境，两者并用、互不替代。当你不确定哪个 skill 契合时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
