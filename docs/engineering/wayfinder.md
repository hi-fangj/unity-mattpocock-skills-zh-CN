Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=wayfinder
```

```bash
npx skills update wayfinder
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/wayfinder)

## What it does

`wayfinder` 接手一个对单个 agent session 来说太大的 effort——被 fog 包围，从这里到目标的路还看不见——并把它绘制成你 issue tracker 上的一张由 **decision tickets** 组成的 **shared map**，然后一次解决一个，直到路径清晰。它**做 planning，不做执行**：每个 ticket 解决一个 decision——一个需要敲定的问题，而不是一个待执行的 build 切片——而当在有人去构建这东西之前再没有什么需要决定时，map 就完成了——所以它产出的是 decisions，而不是 deliverables。

## When to reach for it

你通过输入 `/wayfinder` 调用它——agent 不会自行触发。

当一个 effort **超出单个 agent session 所能容纳的范围**、并且通往其 **destination** 的路线仍然有 fog 时使用它——你能感觉到工作的形状，却还无法把它写成一份 spec 或 plan。要把一条*已经清晰*的 thread 变成 spec，使用 [to-spec](https://aihero.dev/skills-to-spec)；要把一个已经理解的 plan 切成可构建的 tickets，使用 [to-tickets](https://aihero.dev/skills-to-tickets)。Wayfinder 位于两者上游：它是当 fog 太多、无法直接写 spec 时你所运行的东西。

## Prerequisites

Map 及其 tickets 位于 repo 的 issue tracker 上，因此 wayfinder 需要 [setup-matt-pocock-skills](https://aihero.dev/skills-setup-matt-pocock-skills) 铺设的 tracker wiring——它会播种一个 “Wayfinding operations” 小节，描述 map、child tickets、blocking 和 frontier queries 在 GitHub、GitLab 或 local-markdown 上如何表达。缺少那份文档时，wayfinder 默认使用 local-markdown map。

## The map is an index, fog is the frontier

**Map** 是一个单一的 `wayfinder:map` issue，其 tickets 是它的 child issues——一个整个团队都能关注的共享 URL。它是一个 **index, not a store**：每个 decision 恰好存在于一个地方（它的 ticket），map 只做摘要和链接，绝不复述。一个 session 以低分辨率加载 map，并按需放大到各个 ticket。

在 live tickets 之外是 **fog of war**——你能看出即将到来、却还无法精确描述的 decisions。判断某样东西是 ticket 还是仍是 fog 的标准，是你现在能否*精确地说出问题*，而不是你能否回答它。解决一个 ticket 会清除它前方的 fog，把现在可以明确说明的内容 **graduate** 成新的 tickets。**Frontier** 是那些 open、unblocked、unclaimed 的 tickets——已知世界的边缘——它正是 tracker 的 native blocking 在视觉上呈现的东西，因此你无需打开 map 就能看到什么是可领取的。Fog 只聚集在*通往* **destination** 的方向上；越过它的工作被判定为 **out of scope**，被关闭，永不 graduate。

每个 ticket 要么是 **HITL**（human in the loop——grilling、prototype），要么是 **AFK**（agent 独自进行——research）；一个 HITL ticket 只能通过一次 live exchange 来解决，因此 agent 绝不自问自答。Research 仍然是一个真实的 ticket——一个下游 decisions 所悬挂的共享 blocker——但因为它是 AFK，session 不会停下来阅读：它会启动一个 `/research` **subagent** 并行地把这个 ticket 烧掉，保持 frontier 快速，并把 findings 捕获到一个一次性的 `research/<name>` branch 上。

## It's working if

- 命名 **destination** 是第一件事——在任何 ticket 存在之前——因为它固定了每个 ticket 据以衡量的 scope。
- 一个 map 就是一个 `wayfinder:map` issue；tickets 是它的 child issues，以 **name** 引用，绝不是一个光秃秃的 `#42`。
- 一个 session 最多解决**一个 ticket**（research tickets 除外），把答案记录为一条 resolution comment，关闭该 ticket，并在 *Decisions so far* 追加一行 pointer。
- 如果 opening grill 没有浮现出 **fog**，它就停下来，并告诉你这段旅程小到可以跳过 map。

## Where it fits

`wayfinder` 是一个 big-idea **on-ramp**：一个太大、太有 fog、无法一口气写出 spec 的 effort，会生成一张已清除的 decisions map，然后汇入 main build flow。当 fog 被推回、路径清晰后，交接给 [to-spec](https://aihero.dev/skills-to-spec) 来安排这个多 session 的构建（或者，如果 effort 结果很小，就直接 implement）。它依靠 [grilling](https://aihero.dev/skills-grilling) 和 [domain-modeling](https://aihero.dev/skills-domain-modeling) 来解决各个 ticket，并依靠 [prototype](https://aihero.dev/skills-prototype) 和 [research](https://aihero.dev/skills-research) 来处理需要它们的 ticket 类型。当你不确定哪个 skill 或 flow 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你引路。
