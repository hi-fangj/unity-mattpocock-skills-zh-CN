Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=setup-matt-pocock-skills
```

```bash
npx skills update setup-matt-pocock-skills
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/setup-matt-pocock-skills)

## What it does

`setup-matt-pocock-skills` 教会一个 repo：engineering skills 在其中应该如何运作——issues 放在哪里、triage labels 叫什么名字、domain docs 放在哪里，以及 Unity repository 应如何编译和验证——并把这些答案记录为其他 skills 会读取的 **config**。

它写的是 config，而不是把行为硬编码进去。这个 skill 是一次性 bootstrap，内容从真实 repo 中发现（`git remote`、已有 labels、`CONTEXT.md`、Unity version、assemblies、generated ownership 与 build commands），并与你确认，而不是靠猜。它是 prompt 驱动的——探索、呈现它发现的内容、确认、然后写入——而不是一个确定性的 scaffold。

## When to reach for it

你通过输入 `/setup-matt-pocock-skills` 调用它——agent 不会自行触发。

**每个 repo 使用一次，在首次使用任何其他 engineering skill 之前**。如果 [triage](https://aihero.dev/skills-triage)、[to-spec](https://aihero.dev/skills-to-spec) 或 [to-tickets](https://aihero.dev/skills-to-tickets) 开始猜测你的 issues 放在哪里，或套用并不存在的 labels，说明它们还没有在这里完成配置。只有在切换 issue tracker 或想从头来过时才重新运行——日常的微调只是编辑 `docs/agents/*.md`。

## The three decisions

它为每一项都先给出一个推荐答案，你可以一个词就接受，并跳过它已经能推断出的内容——所以大多数运行只是几次快速确认：

- **Issue tracker** — 工作在哪里被跟踪，这样 `triage`/`to-spec`/`to-tickets` 才知道该调用 `gh`、`glab`，还是在 `.scratch/` 下写 markdown，抑或遵循你描述的某个 workflow。GitHub、GitLab、local markdown，或其他。（它会提议与你 `git remote` 匹配的那一个。）
- **Triage labels** — 仅在安装了 `triage` skill 时才询问，而且只问一句：保留默认 labels（`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`）吗？只有当你的 tracker 已经在使用其他名字时才说 no，这样 `triage` 就会套用真实存在的 labels，而不是创建重复项。
- **Domain docs** — 假定是 single-context（根目录一个 `CONTEXT.md` + `docs/adr/`），这适用于几乎所有 repo；只有当它发现 monorepo 信号时，才会提出 multi-context map。
- **Unity development** — 当 `ProjectSettings/ProjectVersion.txt` 存在时自动运行，记录 assembly/layer boundaries、generated-code ownership、最小 compiler checks、Unity-facing scenarios 与 high-risk paths。

输出是 `docs/agents/` 下的一组文件——`issue-tracker.md`、`domain.md`，安装了 `triage` 时的 `triage-labels.md`，以及 Unity repository 的 `unity-development.md`——外加一个指向它们的 `## Agent skills` 块，写入 repo 已经在使用的 `CLAUDE.md` / `AGENTS.md` 其中之一。这些文件就是整个工具包其余部分所立足的共享基底。

## It's working if

- `issue-tracker.md` 和 `domain.md` 落在 `docs/agents/` 下（安装了 `triage` 时还有 `triage-labels.md`），并且你的 `CLAUDE.md` 或 `AGENTS.md` 中出现了一个 `## Agent skills` 小节。
- Unity repository 还会得到 `docs/agents/unity-development.md` 与 `$unity-development` pointer，其中记录真实 assemblies 与 verification paths。
- 它提议的 tracker 与你真实的 `git remote` 匹配，labels 与你 repo 中已经存在的字符串匹配。
- 之后，`triage` 和 `to-tickets` 会在正确的地方、用正确的 labels 行动，而不是询问或猜测。

## Where it fits

`setup-matt-pocock-skills` 是一个 **run-once setup**——整个 engineering 套件所立足的基础，而不是你会重复执行的步骤。它的邻居是那些读取它所写内容的 skills：[triage](https://aihero.dev/skills-triage)，因为它套用在这里配置的 label vocabulary；以及 [to-spec](https://aihero.dev/skills-to-spec) / [to-tickets](https://aihero.dev/skills-to-tickets)，因为它们发布到在这里配置的 issue tracker。先运行它；下游的一切都假定它已经运行过。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你引路。
