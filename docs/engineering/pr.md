Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=pr
```

```bash
npx skills update pr
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/pr)

## What it does

`pr` 定义 pull request body 的形态：展示变更的 **Summary**、它确实可用的 **Evidence**，以及对落地风险有多大的 **Merge Danger** 判断。它是一个 format reference，不是 workflow：不 push branch、不打开 PR、也不决定 PR 里放什么，只告诉 [agent](https://www.aihero.dev/ai-coding-dictionary/agent) 写 body 时应该长什么样。

Summary 是一个 visual，不是一段文字。默认的 PR body 用 prose 描述 diff；这一份选择能把关键点讲清楚的**最小视图**（pseudocode、call tree、component tree、file tree、Mermaid diagram 或成形 diff），周围文字保持简短。Reviewer 反正已经打开了 diff，body 的职责是让他在读之前先看到形状。

## When to reach for it

输入 `/pr`，或 agent 每次撰写 PR body 时自动使用它。

| 你的情境 | 用什么 |
| --- | --- |
| branch 已就绪，需要一个 reviewer 能快速扫读的 body | `pr` |
| 代码写好但还没人审查 | 先 [code-review](https://aihero.dev/skills-code-review)，再 `pr` |
| PR 已打开、review comments 正在回来 | 本集合暂无对应 skill；`pr` 只写 body |

## The template

三个 section，按此顺序：**Summary**（一个或多个小 visual，每个放在它支撑的短文本旁，只保留 reviewer 需要的 calls、files、props 和 boundaries）、**Evidence**（before 与 after：变更可视觉呈现且环境支持时 screenshot 最有力，否则展示那条确切的、先失败后通过的 test，用 pseudocode 写出，或变化的 console output）、**Merge Danger**（**one-way door** 还是 **two-way door**，以及 **blast radius**：如果变更错了可能破坏什么）。

Door call 是核心思想：它把“这个可以安全 merge 吗”从直觉变成 reviewer 可以反驳的明确主张，也告诉他应该把 [human review](https://www.aihero.dev/ai-coding-dictionary/human-review) 花在哪里。Blast radius 小的 two-way door 扫一眼即可；one-way door 要慢慢读。

skill 不打开 PR，也不处理返回的 review comments，body 也不会随 PR 变化自动更新；PR 大改之后让 agent 重写一次 body 即可，它会自动套用同一形态。仓库自带的 PR template 与它是两套竞争指令，要在 repo 的 agent docs 里裁决优先级。对 agent 自己给出的 door call 保持警惕：写变更的 agent 正是在给自己打分，对“two-way、blast radius 小”的结论要多看一眼，并把你的 repo 一贯视为 one-way 的变更（schema migrations、对外发布、删除操作）写进 agent 会读到的文档。

## Where it fits

当构建以 pull request 发布时，`pr` 位于 review 与 retro 之间：`to-spec → to-tickets → implement → code-review → pr → retro`。它是 model-invoked 的，所以 chain 之外 agent 任何时候写 PR body 都会自动套用这一形态。

[code-review](https://aihero.dev/skills-code-review) 在它之前运行，PR body 应该描述一个已经被审查过的 diff；[implement](https://aihero.dev/skills-implement) 产出 body 所描述的 commits；[implement-spec](https://aihero.dev/skills-implement-spec) 在 tracker 通过 PR 收尾工作或你要求时会打开 draft PR。当你不确定哪个 skill 契合时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
