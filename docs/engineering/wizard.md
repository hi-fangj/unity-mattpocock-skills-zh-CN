Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=wizard
```

```bash
npx skills update wizard
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/wizard)

## What it does

`wizard` 生成一个交互式 bash script，一步一步引导人完成手动流程——接入第三方服务、运行一次性 migration、把项目从 state A 移到 state B。它打开每个 URL，说明要点击和复制什么，捕获返回的内容，并把它写进 `.env` 文件和 GitHub Actions secrets。

[agent](https://www.aihero.dev/ai-coding-dictionary/agent) 写 script；它从不运行 script。运行的是你，在你自己的机器上。所以 wizard 不是一份你要遵循的说明清单——它是一个驱动流程并持有状态的程序，你的部分是点击、粘贴和按 Enter。

## When to reach for it

你可以输入 `/wizard`，agent 也能自己伸手用它。当它碰到必须由你完成的步骤——一个它无法铸造的 key、一个它无法点击的 dashboard——它给你构建一个 wizard，而不是把说明写进 chat，让它们滚出视野。

当挡住你的下一件事是穿越 dashboard 的旅程时使用它：

| 情境 | Wizard 做什么 |
| --- | --- |
| 新 dev 需要在 app 启动前配置六个服务 | 按顺序打开每个 dashboard、捕获 keys、写进 `.env` 和 CI |
| 一次性 migration 需要按特定顺序拨动开关 | 把不可逆步骤编排在 confirmation gates 后面 |
| 项目必须一次性从 state A 移到 state B | 走完 transition，并报告它无法完成的部分 |
| 你正要把那些步骤写进 README | 改为写一个可执行的版本，它不会那么安静地腐烂 |

不要用它来 *decide* 构建什么；那是 [grill-with-docs](https://aihero.dev/skills-grill-with-docs) 和 [to-spec](https://aihero.dev/skills-to-spec) 的工具。

## Prerequisites

生成一个不需要任何前提。它写出的 wizard 运行在 bash 上，并在 stage 设置 GitHub secret 或 variable 时使用 `gh`。如果 `gh` 缺失或未认证，那个 stage 变成 warning，收尾 summary 告诉你该手动设置什么，而不是让运行失败。

## Stages

**Stage** 是一个屏幕上的一项聚焦任务。Script 在 stages 之间清空终端，所以溢出屏幕的 stage 会丢失滚出视野的部分。你按依赖顺序编写 stages，并设置 `TOTAL_STAGES`，它驱动进度显示。

Scoping 在写任何一行之前发生。[skill](https://www.aihero.dev/ai-coding-dictionary/skill) 读取 repo 而不是凭空发问：`.env*`、`docker-compose*`、framework config，以及 `.github/workflows/` 里每一处 `secrets.*` / `vars.*` 引用——每一个都是 wizard 必须产出的值。然后它向你展示有序的 stage 列表供确认，只有在那之后才把每个 stage 映射到人遵循的精确路径（"Dashboard → Developers → API keys → Reveal test key → copy"）。在它不知道当前 UI 的地方，它问你或查文档，而不是编造点击。

对于每个捕获的值，scoping 决定它落在哪里：

| Destination | 何时 |
| --- | --- |
| 只写 `.env` | 本地 dev 需要，CI 不需要 |
| GitHub secret | CI 读取它，而且它敏感 |
| GitHub variable | CI 读取它，而且它是公开的 |
| `.env` 和 secret 都写 | 本地 dev 和 CI 都需要 |
| 哪里都不写 | Stage 是纯操作——拨动的开关、升级的 plan |

## The template already solves the UX

[template](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/wizard/template.sh) 已经交付整套体验：带剩余时间的进度、confirmation gates、含 WSL 的跨平台 URL 打开、secrets 的隐藏输入、幂等的 `.env` upsert、`gh secret` / `gh variable` 写入，以及它不得不跳过的所有内容的收尾 summary。`STAGES` 标记之上的所有东西是固定 library，在每个 wizard 里都相同，从不手工编辑。这种一致性正是重点。你的工作只是确定流程范围并编写它的 stages。

写 wizard 的 agent 从不端到端运行它，因为它会打开浏览器并等待人类输入。它改为静态验证：`bash -n`、可用时 `shellcheck`，以及一次 trace——每个值都落到 scoping 所说的地方，每个 `set_secret` 名称都匹配 CI 中真实的 `secrets.*` 引用。相应调整你的预期——第一次运行是你的，而那次运行就是测试。

## Ephemeral by default

| 你拥有什么 | 怎么处理 script |
| --- | --- |
| 一次性 migration、个人 setup、你永远不会重复的 transition | 保存到 scratch 或 `scripts/` 路径，运行，删除 |
| 下一个在 repo 上的人也会需要的 setup 路径 | 提交它并从 README 链接过去，这样他们运行 script 而不是重新问 agent |

## Common questions

**我的 API keys 会进到 model 的 context 里吗？**

不会。Agent 写 script；它不运行 script。你自己运行 script，它用隐藏的终端输入捕获 key，并直接写进 `.env` 或 `gh secret`。Wizard 是 CLI，model 没有连接它。一个 caveat：那适用于 wizard 在运行时捕获的值。如果你在 scoping 流程时把 key 粘贴进 chat，它就像任何其他粘贴的文本一样进入 [context](https://www.aihero.dev/ai-coding-dictionary/context)。

**我打错了一个值，能回去改吗？**

运行中途不行。没有 back button——stages 向前跑，stage 3 答错意味着 Ctrl-C 重跑。重跑按设计很便宜：任何已写进 `.env` 的值都会被作为 default 回馈，所以已答对的 stages 按 Enter 穿过，只重打错的那个。这是发布周出现且之后再没关闭的问题："loved it! One thing though — is there a way to go back and correct what you've entered?"

还有一个相关的 open bug。`ask` prompt 里的方向键会插入 `^[[D` / `^[[C` 而不是移动光标，因为 prompt 用的是 `read -r` 而不是 Readline（[issue #741](https://github.com/mattpocock/skills/issues/741)）。Backspace 有效；方向键无效。删除回到错误处，而不是把光标移进它。

**它知道我设置了什么吗？**

部分知道，而且比发布时的反应假设的少。它在问之前读取 repo——你的 `.env` 文件、`docker-compose`、framework config、CI 里的 `secrets.*` 引用——所以它 scoping 到真正缺失的值，而不是像 README 那样从零开始。它不做的是检查第三方服务。如果 key 已经存在于你的 `.env`，wizard 会把它回馈出来，Enter 保留；如果你已经创建了 Stripe account 但从没保存 key，wizard 仍会送你去 dashboard 取它。

**它在 workflow 里位于哪里——grilling 和 spec 之后？**

哪里都不特别在。它是 standalone，不是 chain 步骤。常见猜测是 `/grill-with-docs → /to-spec → /wizard`，那个顺序没问题，但触发器是出现一个手动流程，它可以在任何时间点发生：开始之前、构建中途、发布很久之后。它也能当 discovery tool——scoping 会暴露任务隐藏的前提，比如三个你没想过的 API keys，在 commit 到工作之前。

**它在 Claude Code 之外能用吗？**

Artifact 无条件能用：它是纯 bash script，不在乎是哪个 [harness](https://www.aihero.dev/ai-coding-dictionary/harness) 生成的。Skill 本身是 model-invoked，所以它在任何地方都被列出——在 Claude Code 里输入 `/wizard`，或在 Codex 里输入 `$wizard`，或只是描述你卡住的 setup。Being model-invoked 也让它避开 [#693](https://github.com/mattpocock/skills/issues/693)，那里 Claude 的 desktop 和 web surfaces 会从 [model](https://www.aihero.dev/ai-coding-dictionary/model) 的 listing 中丢弃 *user-invoked* skills，并把它们报告为未安装。

**它以前不是 user-invoked 吗？**

是的。它现在是 model-invoked，所以 agent 碰到必须由你完成的步骤时，会不提示地伸手用它。你以前能做的没有一样停用——model-invocation *增加* agent 的 reach，从不移除你的，所以 `/wizard` 的行为和以前完全一样。变的是它退役的 failure mode：agent 在构建中途撞上 credentials 墙，把六条编号步骤倒进 chat 让你手工照做。

**它以前在 `in-progress/` 里——现在在哪？**

`engineering/`，从 v1.2 起。它从 beta bucket 毕业，现在随 plugin 一起发布，所以它和其余升级集合一起到达，而不需要单独安装。毕业没有改变它的行为。

## It's working if

- 你看到有序的 stage 列表，以及每个产出的值，并被要求确认——在任何 script 存在之前。
- 每个 URL 都在索取该页面上的值之前被打开。你永远不会被要求粘贴还没被送去取的东西。
- Secrets 盲打输入。没有敏感内容 echo 进你的 scrollback。
- 每个 stage 占一个屏幕。你还需要的没有滚出视野。
- Ctrl-C 然后重跑会从你停下的地方接上，把已保存的值作为 defaults 提供。
- 最后屏列出它写了什么，并单独列出它无法完成、你必须手工收尾的内容。

## Where it fits

`wizard` 是一个随时伸手可用的 standalone，坐在 automation 停止、人类必须点击的线上。它最近的邻居是 [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills)，因为两者都是为了让 repo 进入可工作状态而存在——那个配置的是这套 skills，而 `wizard` 为其他一切生成 setup 路径。它也和 [implement](https://aihero.dev/skills-implement) 配对：当构建落地一个需要 credentials 或手动 cutover 的 feature 时，wizard 就是 human 那一半完成的方式。当你不确定哪个 skill 适合当下时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
