Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=writing-for-agents
```

```bash
npx skills update writing-for-agents
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/writing-for-agents)

## What it does

`writing-for-agents` 是你写 agent-facing 文档所对照的 reference——一个 skill、一份 `AGENTS.md` / `CLAUDE.md`、一份 [spec](https://www.aihero.dev/ai-coding-dictionary/spec)、一个 runtime prompt、一个 README、任何 [agent](https://www.aihero.dev/ai-coding-dictionary/agent) 会读的 doc。打包方式不同；写作方式不同：同一套杠杆让每一份都可预测，所以 agent 每次运行都走相同的 *process*，而不是产出相同的 output。

它的默认动作是删除，不是解释。让 agent 为另一个 agent 写说明，它会用大部分词解释 [model](https://www.aihero.dev/ai-coding-dictionary/model) 已经知道的东西——那些行每一行都是 **no-op**，花 [context](https://www.aihero.dev/ai-coding-dictionary/context)、不改变任何 behavior。这个 reference 是找出它们的镜头，这就是为什么它在你已有的文档上至少和在一张空白文件上一样常被拿出来用。

它在 v1.1 之前叫 `writing-great-skills`。Rename 对应它骨子里一直是什么：几乎没有 skill-specific 的部分。Skill-only 的 mechanics——frontmatter、model-invoked 与 user-invoked 的选择、router skills——被 disclose 到链接的 `SKILL-MECHANICS.md`，只有当你面前这份文档是 skill 时才读它。

## When to reach for it

输入 `/writing-for-agents`，或在创建或编辑 skill、修改 `AGENTS.md` 或 `CLAUDE.md` 时由 agent 自己伸手用它。

手工使用它处理 agent 读的一切其他东西：你的 docs、specs 和 [tickets](https://www.aihero.dev/ai-coding-dictionary/ticket)、system 和 [AFK](https://www.aihero.dev/ai-coding-dictionary/afk) prompts。测试是一个问题——agent 会读这个吗？——而文档如何到它面前并不重要，无论是 pointer 点名它、人类粘贴它，还是它只是坐在 repo 里。要首先搞清楚 codebase 实际包含什么，用 [grill-with-docs](https://aihero.dev/skills-grill-with-docs)——这个 reference 管的是文档读起来如何，不是它知道什么。

## The two loads

整个 reference 围绕的想法是每份文档和 pointer 花费的一对预算：

- **Context load**——always-loaded material 对 agent window 的代价：一行 `AGENTS.md`、一个 skill description、任何每 [turn](https://www.aihero.dev/ai-coding-dictionary/turn) 都坐在 context 里的东西，无论它是否触发。
- **Cognitive load**——对你的代价：存在哪些文档，以及何时伸手拿每一份。你是索引。不是要最小化的成本——它是 human agency 的价格。

一旦你用这两个 loads 思考，大部分写作决定——拆或不拆、inline 或 disclose、point 或 push——都变成在不同地方做出的同一个交易。

## The levers

- **[Context pointers](https://www.aihero.dev/ai-coding-dictionary/context-pointer)**——context 中持有的 reference，点名 context 之外的 material，并编码何时到达它。Skill description 和命名某份 doc 的 `AGENTS.md` 一行是同一个东西；决定 agent 到达它的可靠程度的是 pointer 的 *wording*，不是它的 target。
- **Information hierarchy**——从 in-file step、到 in-file reference、到 pointer 后面 disclosed reference 的阶梯。**[Progressive disclosure](https://www.aihero.dev/ai-coding-dictionary/progressive-disclosure)** 是往阶梯下走的动作，让顶层保持可读。
- **Completion criteria**——每个 step done-condition 的 clarity 和 demand，以及那个 demand 驱动的 **legwork**；这是对 **premature completion** 的防御。
- **Leading words**——已经在 model pretraining 里的紧凑概念（*tight*、*red*、*tracer bullet*），agent 运行文档时用它思考。它双重锚定：正文里的 execution，pointer 里的 invocation。
- **Pruning**——single source of truth、relevance，以及逐句应用的 no-op test，用来对抗 **duplication**、**sediment** 和 **sprawl**。

## Common questions

**`/writing-great-skills` 去哪了？**
它就是这同一个 skill，在 v1.1 改名。从业者在名字赶上之前很久就已经把它指向 `AGENTS.md`、docs、specs、tickets 和 runtime prompts；structure、leading words 和 pruning 被证明是任何 agent 会读的文本的工艺。没有 alias——用新名字重新安装。

**"Writing for agents"——所以是 agent 来写？**
反过来。你是作者；agent 是读者。那是这类文本的全部难点：你在为一个已经读过一切东西的读者写作，所以解释是浪费，precision 是全部工作。

**我不能直接让 agent 替我写吗？**
可以，而它会产出啰嗦的东西。不被干预时，model 会解释它已经知道的东西，而它不会自己应用 no-op test 或伸手拿 leading word。在草稿上使用这个 reference——review pass 是它大部分价值落下的地方。

**我让 agent 删减一份文档，它把功能剪掉了。**
被要求 "streamline" 的 agents 会为长度优化，因为长度是它们能看到的东西。No-op test 是 behavioral 的，不是 aesthetic 的：删掉那行，问 agent 的 behavior 是否变了。当一句失败时，删除整句，而不是从它里面修剪词——并对它的分歧用运行文档来解决，而不是争论。

**我怎么知道它完成了？**
当它有效，而且你再也找不到 duplication、sediment 或 no-ops 时。这里没有自动化 eval；检查是手动运行，加上把 failure-mode vocabulary 当作 diagnostic。当一份文档行为不端时，那个 vocabulary 也是 repair kit——先命名 failure mode，然后修复它。

**这应该住在 `CLAUDE.md` 还是别处？**
问你想付哪个 load。`CLAUDE.md` 无条件加载进每个 [session](https://www.aihero.dev/ai-coding-dictionary/session)；pointer 后面的 material 在触发之前只花 pointer 自己那一行。任何十种 context 里只有一种适用的东西，其余九次都在付 context load。

**我需要为每个新 model 重写文档吗？**
大多不需要，而且 over-fitting 到单个 model 是它自己的陷阱。为新 model 更新通常是又一次 no-op pass，而不是重写。

**我的 skill 只在它来自的那个精确任务上有效。**
常见路线——做一次工作，然后让 agent 把它写成一个 skill——过度索引那次运行，exemplars 会太 specific。把那次运行留作 evidence，然后刻意抽象：剥离属于那个 repo 和那些文件的东西，为那一类任务写作。

**英语不是我的第一语言。我会失去 leading-word 优势吗？**
不会——找到把最多 behavior 塞进最少 [tokens](https://www.aihero.dev/ai-coding-dictionary/token) 的词，是 reference 替你做的活。它是这份 reference 存在的理由之一。

## It's working if

- 文档变好就变短，而你对剩下多少感到惊讶。
- 你能指向一个 leading word，并看到它在不止一处干活。
- 没有任何东西以任何形式被陈述两次。Duplication 是最可靠的、文档从未被测试过的迹象。
- 只有某个 branch 需要的 reference 坐在 pointer 后面，而不是在主文件里。

## Where it fits

这是随时伸手可用的 standalone reference。它在 chain 里没有邻居，因为它坐在整套之下，而不是任何单个 skill 旁边：这里的每个 skill 都是对照它写的，而其他 skills 留下的文档——`CONTEXT.md` 和它的 ADRs、一份 spec、一份 ticket——一旦 agent 要读它们，就正是它所管束的文本。当你不确定哪个 skill 或 flow 适合任务时，[ask-matt](https://aihero.dev/skills-ask-matt) 会带你过完整套。
