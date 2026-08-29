---
name: ask-matt
description: 询问当前情境适合哪个技能或流程；它是本仓库所有 skills 的路由器。
disable-model-invocation: true
---

# Ask Matt

你不需要记住每个 skill，所以直接问。

**Flow** 是穿过 skills 的一条路径。大多数路径沿着一条 **main flow** 前进，两个 **on-ramps** 会并入它。其他内容要么是 standalone，要么是在下层运行的 vocabulary layer。

## Unity projects

当 `ProjectSettings/ProjectVersion.txt` 表明这是 Unity repository 时，把 `$unity-development` 与 repository 的 Unity development document 作为本 router 下层的 execution reference。此分支中：

- 使用 Unity feedback loop，不要默认 `/implement` 与 `/tdd` 的通用命令行测试假设；
- 使用 `/prototype`、`/diagnosing-bugs`、`/code-review` 与 `/resolving-merge-conflicts` 的 Unity branches；
- 只有变更或用户明确要求时，才运行 broad tests、player/resource builds、review workflows 与 commits。

## The main flow: idea → ship

这是大多数工作的路线：你有一个想法，并希望把它构建出来。

1. **`/grill-with-docs`** - 通过访谈打磨想法。在 **working directory** 中工作时从这里开始：它是 stateful 的，会把学到的内容保存在 `CONTEXT.md` 和 ADRs 中。（没有 working directory？用 `/grill-me`，见 Standalone。两者都运行同一个 `/grilling` primitive；`grill-with-docs` 是会留下文档痕迹的版本，只要有 repo 可以留下痕迹，它总是两者中更好的选择。）
2. **分支：能否在对话中解决所有问题？** 如果某个问题需要可运行的答案（state、business logic，或必须亲眼看到的 UI），就通过 prototype 绕行，并用 **`/handoff`** 在两个方向桥接（prototype 住在自己的目录里，这正是 `/handoff` 的用途，见 Phase boundaries）：
   - **`/handoff`** 导出，然后基于该文件打开 fresh session；
   - **`/prototype`** 用 throwaway code 回答问题；
   - **`/handoff`** 把学到的内容带回来，并在原始 idea thread 中引用它。
3. **分支：这是 multi-session build 吗？**
   - **是** -> **`/to-spec`**（把 thread 变成 spec），再用 **`/to-tickets`** 拆成 tracer-bullet tickets，每个 ticket 声明 **blocking edges**。Local tracker 在 `.scratch/<feature>/issues/` 下每 ticket 一个文件，手动按 blockers-first 处理；真实 tracker 用 native blocking links，因此 blockers 已完成的 ticket 都可领取。每 ticket 启动一次 **`/implement`**，并在 tickets 之间 **`/clear`** context。每个 ticket 都是 self-contained 的，所以上一个 ticket 的 context 可以丢弃。
   - **否** -> 就在同一个 context window 里运行 **`/implement`**。

   无论哪种方式，**`/implement`** 都会在内部驱动 **`/tdd`** 构建每个 issue：一次一个 red-green slice；然后用 **`/code-review`** 收尾，对 diff 做 Standards + Spec 双轴 review，再提交。只想在没有完整 spec 的情况下 test-first 构建一个具体 behavior 时，单独用 **`/tdd`**；想按固定点 review branch 或 PR 时，单独用 **`/code-review`**。

### Context hygiene

步骤 1 到 3 要留在 **同一个未中断的 context window** 中；在 `/to-tickets` 之后才 compact 或 clear，这样 grilling、spec 和 tickets 才能建立在同一组思考之上。之后每个 `/implement` 都从 fresh session 开始，只基于对应 ticket 工作。

限制来自 **[smart zone](https://www.aihero.dev/ai-coding-dictionary/smart-zone)**：在该窗口（最新模型大约 150k tokens）内，模型还能保持敏锐推理。如果 session 在 `/to-tickets` 前接近这个区间，不要硬撑降级状态；在最近的 phase boundary 用 **`/compact`**，然后继续（见 Phase boundaries）。

## On-ramps

起点会生成工作，然后并入 main flow。

- **Bugs 和 requests 堆积** -> **`/triage`**。它通过 triage roles 推进 issues，并产出 agent-ready issues，之后由 **`/implement`** 领取。

  Triage 只用于 **不是你创建的** issues：bug reports、incoming feature requests，以及任何原始进入的内容。`/to-tickets` 产出的 tickets 已经是 agent-ready，**不要再 triage 它们**。

- **Something's broken** -> **`/diagnosing-bugs`**。用于难处理的问题：第一眼看不出的 bug、间歇性 flake、夹在两个 known-good states 之间的 regression。它在拥有 **tight feedback loop** 前拒绝空想，也就是一个已经能在 _这个_ bug 上变红的命令；然后用 regression test 修复。如果复盘发现真正问题是没有好 seam 能锁住 bug，它会把后续交给 **`/improve-codebase-architecture`**。

- **巨大而模糊的 effort：greenfield project 或巨大 feature build，一个 session 装不下** -> **`/wayfinder`**，这是这里认知负担最重的 flow。当从当前位置到 destination 的路还看不见时，它在 issue tracker 上绘制 **decision tickets** 的 **shared map**，逐个解决，产出 **decisions, not deliverables**，直到 fog 被推开、路径清晰。`/grill-with-docs` 用于一个 session 能装下的想法，wayfinder 用于装不下的想法；它更慢、更密集，所以只应留给确实如此的 effort，绝不要用于范围明确的 feature。

  Map 清晰后，**它会 hand off，而不是 build**：在 **`/to-spec`** 处并入 main flow，把 map 中相互链接的 decisions 收束成可构建计划，然后照常使用 `/to-tickets` 和 `/implement`。让 map 直接循环进入 `/implement` 会跳过这次收束并丢掉相互链接的细节；只有当 effort 后来发现确实很小时，才直接进入 `/implement`。

## Codebase health

这不是 feature work，而是维护。

- **`/improve-codebase-architecture`** - 有空时运行，保持 codebase 适合 agents 操作。它会暴露 **deepening opportunities**；选择其中一个会 _生成一个 idea_，可以带入 main flow 的 `/grill-with-docs`。它是负责找候选项的 survey；**`/codebase-design`**（见下文）是你设计已选候选项时使用的工作台。

## Vocabulary underneath

两个 model-invoked references 在其他 skills 下层运行，分别是自己词汇的 single source of truth。问题在于**词语**而不是流程时直接用它们；也可以让上面的 skills 自动拉起它们。

- **`/domain-modeling`** - 打磨项目的 _domain_ language：挑战模糊术语、解决 overloaded word（例如一个 "account" 承担三件事）、把难以逆转的决策记录为 ADR。它是 `/grill-with-docs` 用来保持 `CONTEXT.md` glossary 干净的主动纪律。
- **`/codebase-design`** - deep-module vocabulary（module、interface、depth、seam、adapter、leverage、locality），用于设计 module 的 _shape_：把大量 behavior 放在 clean seam 上的小 interface 后面。`/tdd` 和 `/improve-codebase-architecture` 都使用这套语言。

## Phase boundaries

**Phase** 是 session 内的一块工作：grilling、implementation、QA。在两者之间的 **boundary** 处，你有五个选项，而在这张地图上，在它们之间选择是最模糊的决定：

- **Continue**：留在原地。零成本，零损失。
- **`/clear`**：清空 window，当这里的一切对接下来要做的事都不重要时。
- **`/handoff`**：写一个 portable markdown file。用途很窄：只用于 **new harness**、**new directory**、**colleague**，或在 **mid-phase** 分叉一个 side task。它买到的是 portability。
- **Subagent**：把 tightly-scoped 的任务送进自己的 window，拿回一份 report。
- **`/compact`**：压缩当前 context，并用它 seed fresh session。它是决策树底部的 **default**，而不是第一选择。

阅读 [PHASE-BOUNDARIES.md](PHASE-BOUNDARIES.md) 中的有序决策树：五个问题、每个分支背后的推理，以及为什么 primary-source 成本使 **Continue** 成为第一个要被排除的选项。在 **boundary** 处做决定；mid-phase 时，continue 或把剩余工作拆给 subagents。

## Standalone

完全在 main flow 之外。

- **`/grill-me`** - 与 `/grill-with-docs` 一样的持续访谈，但 **stateless**：不在本地保存任何内容，也不构建 `CONTEXT.md`。当你 **不在 working directory** 中时用它：打磨计划、设计、一段写作，任何没有 repo 托底的东西。在 working directory 中就用 `/grill-with-docs`：它运行同样的访谈并留下文档痕迹，所以严格来说它更好。
- **`/grilling`** - 访谈 primitive 本身：rounds、frontier，事实是 agent 的工作而决定是你的。`/grill-me` 和 `/grill-with-docs` 是两个具名入口，`/triage`、`/wayfinder` 和 `/improve-codebase-architecture` 都在内部运行它。只在你想要不带 wrapper 的访谈时才直接用它。
- **`/resolving-merge-conflicts`** - 逐 hunk 处理进行中的 merge 或 rebase conflict，按**意图**解决（追溯到每一边的 primary source，而不是挑行），然后完成操作。它从不运行 `--abort`。Standalone，不在任何 flow 上：当你已经 mid-conflict 时用它。
- **`/prototype`** - 一个小型 throwaway program，用来回答一个设计问题：这个 state model 感觉对吗，或者这个 UI 应该是什么样。Throwaway 是对代码编写方式的约束，不是销毁它的承诺：答案会折叠进真实代码，prototype 本身作为 **primary source** 保留在 main 之外的 `prototype/<name>` branch 上，并由 implementation issue 指向。它是 main flow 第 2 步的绕行，但任何难以纸面解决的 design question 都可以直接用它。
- **`/research`** - 把阅读工作委托给 **background agent**：它对照 **primary sources** 调研问题，然后在 repo 中留下带引用的 Markdown 文件。你可以在它阅读时继续工作。它产出的文件是要带入 main flow 的 `/grill-with-docs` 的东西；research 为思考提供素材，但不取代思考。
- **`/to-questionnaire`** - 当挡住你的东西不在你的脑子里也不在 codebase 里，而在**别人**脑子里时，它写一份 questionnaire 让别人填写。它是 `/grill-me` 的反面：不是就主题访谈你，而是就**发送**访谈你（发给谁、你需要拿回什么），并把问题对准缺口。拿回来的东西是 `/grill-with-docs` 或 `/to-spec` 的素材。
- **`/wizard`** - 用于只有**人类**能做的步骤：provisioning infrastructure、设置 credentials 或 CI secrets、在一个陌生的第三方 dashboard 上点击操作、运行一次性的 migration 或 cutover。它生成一个 interactive bash script，打开每个 URL、捕获每个值，并写入 `.env` 和 GitHub secrets，这样这个流程就不再需要每次都向 agent 重新解释。Model-invoked，所以 agent 一碰到只有你能通过的墙就会自动用它。如果 agent 自己能做，就应该自己做；这是给真正需要人类在 loop 里的场景。
- **`/wait-what`** - 对一条没落地的消息的 corrective。在对话中途、任何其他 skill 内部使用，agent 会用它缺少的上下文、用 `CONTEXT.md` 的词汇、以 plain English 重新表述它刚才说的话。它在事后生效；`/grill-with-docs` 是事前的 cure，因为早期达成共识的 shared language 才能让 jargon 从一开始就不出现。
- **`/teach`** - 使用当前目录作为 stateful workspace，跨多个 sessions 学习一个概念。
- **`/writing-for-agents`** - 编写 agents 消费的文档的 reference：skills、AGENTS.md、被指向的 docs。

## Precondition

**`/setup-matt-pocock-skills`** - 第一次运行 engineering flow 前先执行，用来配置其他 skills 所依赖的 issue tracker、triage labels 和 doc layout。自定义 issue trackers 也可以。
