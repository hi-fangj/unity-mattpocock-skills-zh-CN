Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=to-questionnaire
```

```bash
npx skills update to-questionnaire
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/productivity/to-questionnaire)

## What it does

`to-questionnaire` 把一个你无法独自定夺的 decision 变成 **questionnaire**——一份 Markdown document，你交给唯一握着你所缺之物的那个人，让他们异步填写，或你们俩在会议中一起过一遍。

它 grill 的是 **send**，绝不是 subject。就主题访谈你在这里毫无意义：不知道主题正是你写信给别人的原因。所以它问两件你总能回答的事——这是给谁的，以及你需要从对方那里拿回什么——并把 document 里的每个问题对准这两者之间的 **gap**。

## When to reach for it

输入 `/to-questionnaire` 调用它——[agent](https://www.aihero.dev/ai-coding-dictionary/agent) 不会自己伸手用它。

当 decision 被锁在另一个人脑中的知识上时使用它：一个 client、一个 domain expert、一个拥有业务规则的 exec、一个你不在同一团队的 colleague。你想要哪个 skill 取决于答案实际在哪里：

| 答案在…… | 使用 |
| --- | --- |
| 你自己脑中，未经打磨 | [grill-me](https://aihero.dev/skills-grill-me) |
| Codebase 里 | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) |
| 别人的脑中 | `to-questionnaire` |
| 还没有任何人的脑中——问题需要点什么来反应 | [prototype](https://aihero.dev/skills-prototype) |

常见场景是一场停滞的 [grilling](https://www.aihero.dev/ai-coding-dictionary/grilling) session：浮现出来的一些东西不是你能回答的。在同一对话中运行 `/to-questionnaire`，把这些 questions 带到线下，然后把答案带回来继续。

## The send, not the subject

访谈是两个 exchange，然后它停止。

- **这是给谁的？** 他们的 role、expertise、与你的关系。这确定语气和 document 必须携带多少 context——外部 client 需要 orientation，teammate 不需要。
- **你需要拿回什么？** 你无法独自解决的具体 decisions 或 facts。这成为衡量成品 document 的 checklist：你点名的每一项都配有一个对准它的问题。

此后一切都是 drafting。文件落在当前目录的 `to-questionnaire-<slug>.md`。没有 setup、没有 workspace、没有要配置的东西。

## The document

它被定位为 **discovery questionnaire**——你缺少 context，recipient 握着它——而这个定位驱动它的形状：

- 一行 purpose，点名 ride 在其上的 decision，以及一段短 context，给一个从未进过你脑子的 recipient。
- Questions 按 **most-important-first** 排序，并按主题分组在 headings 下，因为 async 意味着你可能只有一次机会。
- 每个问题一个 idea，绝不 compound，正下方有 answer stub，只有可能被误读的问题才有 *why this matters* 一行。
- 明确允许回答 "I don't know"——被标记的不确定有用；读起来像事实的自信猜测没用。
- 一个收尾 catch-all：还有哪些我们没问、但应该知道的内容？

它刻意不是两样东西。它不是 **branching**——questions 是扁平分组列表，不是那种你答了 A 就跳过 D section 的树。它也不是 **multi-recipient**——一次运行只为一个人产出一份 document。

## Common questions

**它会读我的 grilling session 并从中提取 questions 吗？**
不作为独立步骤。Skill 没有 ingest phase：它问 send，然后 drafting。让它在 grilling session 之后有效的是你在**同一对话**中运行它，所以 [session](https://www.aihero.dev/ai-coding-dictionary/session) 已经在 [context](https://www.aihero.dev/ai-coding-dictionary/context) 里，drafting 可以从中取材。在 fresh session 里启动它，它就不知道 grilling 的任何事——你回答 "what do you need back?" 时要自己重新提供主题。

**缺失的答案不都在同一个人那里。它能按 recipient 拆分吗？**
不能。第一步问的是 *the* recipient，单数，整份 document 的语气和 context 都是冲着他们去的。如果三个人各握着答案的三分之一，就跑三次，每人一次。在单份 document 里按 discipline 或 role 路由 questions 是人们的请求；它不是已发布的样子。

**Questions 相互依赖吗——会根据更早的答案跳过 sections 吗？**
不。依赖式 question 设计被探索过，没有发布。Output 是静态 document：按主题分组、most-important-first、每个 question 都是活的。反对它的理由有道理——[model](https://www.aihero.dev/ai-coding-dictionary/model) 在真实答案之前规划两三个以上的 questions 会规划得很糟，而 branching document 必须在每个答案之前规划全部。

**如果 recipient 也不知道呢？**
Document 告诉他们说出来。"I don't know" 和部分答案被明确要求，被标记的不确定比猜测更有价值，因为含糊的回答和自信但错误的回答，一旦回到你的 context 里看起来一模一样。

**它会把它发到任何地方吗——Slack、issue tracker、email？**
不会。它在当前目录写一个 Markdown file，并告诉你路径。Delivery 是你的：把它贴进 [ticket](https://www.aihero.dev/ai-coding-dictionary/ticket)、丢进 Slack thread、附到 email，或在共享屏幕上打开并现场过一遍。人们四种都手工接通过。

**这不就是 batch 模式的 `/grill-me` 吗？**
不是，而这个区别值得守住。`grill-me` 已经按 **rounds** 问——一次整个 frontier，然后根据你的回答重新计算——所以"把问题一次全给我"的需求在那里已经满足。`to-questionnaire` 关于的是另一条轴：不是 questions 如何投递，而是答案在谁的脑里。自己更快地回答它们是 `grill-me`；把它们从别人那里挖出来是这个。

**我不能直接找 agent 要而没有 skill 吗？**
能，而且很多人在它存在之前就这么做了——`OPEN_QUESTIONS.md` 文件、发给 client 的 spreadsheets、每个未答问题一张 "needs more info" ticket。Skill 给你买两样东西：访谈永远不会漂到 subject 上，以及 document 以非技术 recipient 真能填的形态出来。如果你已经有能用的 house format，诚实答案是你不需要这个。

## It's working if

- 它问 recipient 和你需要拿回什么，然后停止提问。一个关于 subject 本身的问题就是 skill 出轨了。
- 你点名的每一件"what I need back"都能在文件里追溯到一个问题。
- Questions 读起来对准的是 *recipient* 知道的东西，而不是你自己的 open questions 逐字抄下来。
- 你能把文件交给一个没在对话里的人，而他们会知道为什么收到它、什么时候该回复。
- 回来的答案是一轮新 grilling 的可输入素材，而不是一组新 questions。

## Where it fits

`to-questionnaire` 是随时伸手可用的 standalone。它坐在你自己知识的边界上，在那里下一步是另一个人，而不是另一个 skill——最常见于流程中途，当 planning 卡在不是由你决定的东西上时。

它的邻居是 [grill-me](https://aihero.dev/skills-grill-me)，两者按答案在哪里分界：grilling 挖你，questionnaire 挖别人。回来的东西是 raw material——喂进另一轮 grilling，或如果工作朝着 build 去，就喂进 [grill-with-docs](https://aihero.dev/skills-grill-with-docs) 或 [to-spec](https://aihero.dev/skills-to-spec)。当你不确定哪个 skill 适合当下时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
