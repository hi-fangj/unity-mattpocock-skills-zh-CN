---
name: writing-for-agents
description: 编写供 agents 消费的文档。适用于创建或编辑 skills、修改 AGENTS.md 或 CLAUDE.md 时。
---

编写任何 agent 消费的文档的 reference：skill、`AGENTS.md` / `CLAUDE.md`、通过 pointer 到达的 doc。打包方式不同；写作方式不同：同一套杠杆让每一份都可预测，agent 每次运行都走相同的 _process_，而不是产出相同的 output。

当你写的是 skill 时，先读 [`SKILL-MECHANICS.md`](SKILL-MECHANICS.md)，了解 frontmatter、invocation 选择和 router skills。

## Context pointers

**Context pointer** 是 agent context 中持有的一个 reference，它点名某些 context 之外的 material，并编码到达它的条件。Skill 的 description 是其中一种；`AGENTS.md` 中命名某份 doc 的一行是同一个东西。Pointer 的 _措辞_，而不是它的 target，决定 agent 何时到达 material，以及有多可靠。Must-have target 配 weakly worded pointer 就是 variance bug：先磨砺措辞，只有磨砺失败才把 material inline 进来。

Pointer 做两件事：说明 material 是什么，并列出应当触发到达它的 **branches**（branch 是文档处理的独立 case，因此不同运行会走不同路径）。Always-loaded pointer 的每个词每个 turn 都花钱，所以它比正文更需要无情地删减：

- **前载 leading word**：触发工作是在 pointer 这里完成的。
- **每个 branch 一个 trigger。** 只是给同一个 branch 换个名字的 synonyms 是同一个 branch 写了两次；把它们合并，只保留真正不同的 branches。
- **砍掉正文已经携带的 identity。**

## The two loads

你添加的每份文档和 pointer 都花两个预算之一：

- **Context load**：always-loaded material 对 agent window 的代价：一行 `AGENTS.md`、一个 skill description，任何每个 turn 都坐在 context 里的东西，无论它是否触发都在花 tokens 和 attention。
- **Cognitive load**：对人（human）的代价：存在哪些文档，以及何时伸手去拿每一份。Human 是索引。这不是要最小化的成本：它是 human agency 的代价；在 human judgement 重要的地方花它，在不重要的地方拿掉它。

只有通过 pointer 才能到达的 material，以 pointer 自己那一行为代价，逃出 context load；完全没有 pointer 的 material 完全骑在 cognitive load 上。

## Information hierarchy

一份文档由两种内容类型构建，即 **steps**（agent 执行的有序 actions）和 **reference**（按需查阅的 definitions、rules、facts）。它们可以自由混合：全是 steps（一份 recipe）、全是 reference（review 的 rules、这个 skill），或两者都有。核心决定是每一块内容放在 **information hierarchy** 的哪个位置，这是一架按 agent 多立刻需要该 material 排序的阶梯：

1. **In-file step**： 第一层：agent 做什么，按顺序。
2. **In-file reference**： 按需查阅。通常是合理的扁平 peer-set（一份 review 的所有 rules 在同一级），这是良好安排，不是 smell。
3. **Disclosed reference**： 推出去放进单独文件，通过 context pointer 到达，只在 pointer 触发时加载。从同目录的 sibling file 一直延伸到任何地方都存在、任何文档都能指向的 fully external reference。

推得太少，顶层膨胀；推得太多，你藏起了 agent 真正需要的 material。这个张力就是整个决定。

**Progressive disclosure** 是往阶梯下走的动作（出主文件、到 pointer 后面），让顶层保持可读。主要不是 token 优化：它是 hierarchy 被保护的方式。Branching 是最干净的 disclosure 测试：把每个 branch 都需要的内容 inline，把只有部分 branch 会到达的内容推到 pointer 后面。当文档有 steps 时，本应 disclosed 的 in-file reference 会埋掉它们，让注意它们变成抛硬币，这是 variance lever，不只是 legibility 问题。

**Co-location** 是文件内的 companion：阶梯决定一块内容 _往下走多远_，co-location 决定它一旦到那里 _旁边放什么_。把一个概念的 definition、rules 和 caveats 放在一个 heading 下，而不是散开，这样读一部分就会带上它的邻居。测试：文档应该读起来像为 agent 写的文档，分组材料读起来如此；散落材料读起来不像。（与 duplication 不同：那是两处重复同一意义；scattering 是把一个意义碎片化成多处。）

**Sprawl** 是这里的 failure mode：一份文档就是太长，即使每一行都是活的、独有的。Attention 在多余内容上变薄，每一行多出来的一行都要保持 relevance。解药是阶梯：把 reference disclose 到 pointer 后面，并按 branch 或 sequence 拆分，让每条路径只带它需要的。

## Steps and completion criteria

每个 step 都以 **completion criterion** 结束，告诉 agent 工作完成的条件。两个属性使它成为 lever：

- **Clarity**： agent 能区分 done 与 not-done 吗？模糊的边界（"understanding reached"）招来 **premature completion**：在真正完成前结束 step，attention 滑向 _被完成_。仍可见的后续 steps（**post-completion steps**）提供拉力；criterion 的清晰度是阻力。按顺序防守：**先磨砺边界**（本地、便宜）；只有它不可救药地模糊 _并且_ 你观察到赶进度，才通过拆分 sequence 隐藏后续 steps，而且隐藏只在真正的 context boundary 处有效（hand-off 或 subagent dispatch；inline call 会把后续 steps 留在 context 里，什么都清不掉）。
- **Demand**： 它要求多少。"Every modified model accounted for" 迫使彻底工作，而 "produce a change list" 不会。Demand 驱动 **legwork**：agent 在工作的内部所做的挖掘，潜伏在措辞里而不是写成独立 step，而且它不限于 step："every rule applied" 像 "every step done" 绑定 sequence 一样绑定一组扁平 reference，这就是为什么纯 reference 文档也带着 exhaustiveness bar。

最强的 criteria 既可检查又穷尽。

## When to split

把一份文档拆成两份花两个 load 之一，所以只在拆分值得时才拆：

- **By sequence**： 拆分一段 steps，当 post-completion steps 引诱 agent 赶面前那一步时。把它们移出视野会在当前任务上驱动更多 legwork。小心反向：合并 sequences 会把每步的后续 steps 暴露给其后内容，招来 premature completion。
- **By invocation**： skill-specific：见 [`SKILL-MECHANICS.md`](SKILL-MECHANICS.md)。

## Leading words

**Leading word** 是已经存在于 model pretraining 中的紧凑概念，agent 运行文档时用它思考（_lesson_、_fog of war_、_tracer bullets_）。以 token 形式重复，从不以句子形式，它累积一个分布式定义，并用最少的 tokens 锚定一大片 behavior，通过招募 model 已有的 priors。自造词也行，只要定义清楚，但编造的词招募不到 priors：你要为它付出定义 tokens，而 pretrained 词免费给你；先伸手拿已有的词。

它双重锚定。在正文里，_execution_：每次词出现，agent 都伸手拿同样的 behavior，在扁平 reference 内部它把 attention 聚焦到要寻找的一类东西。在 pointer 里，_invocation_：当同一个词存在于你的 prompts、你的 docs 和你的 codebase 时，agent 把这种 shared language 与 material 联系起来，更可靠地到达它。

寻找用 leading words 重构的机会。三处展开的 triad、花一个句子指向一个想法的 pointer，每一段都是乞求塌缩成单个 token 的文字：

- "fast, deterministic, low-overhead" → _tight_（a _tight_ loop）。
- "a loop you believe in" → _red_：模糊的 gate 变成二元可观察状态（loop 在 bug 上 _red_，或不是）。

你赢两次：更少的 tokens，以及让 agent 挂思考的 sharper hook。假设每份文档都带着 leading words 可以退役的 restatements，去找它们。

**Negation** 是这个 lever 旁边的 failure mode：靠禁止来引导，会把被禁 behavior 拖进 context，让它 _更_ 可用，而不是更少。_Don't think of an elephant_，大象就无处不在；否定是被强激活概念压过的弱 modifier，所以禁令半读成"去做那件事"。Prompt 用 **positive**：说出目标 behavior（"write one-line comments"），这样被禁的那个永远不会被说出。禁令只有在它是你无法正面表述的硬 guardrail 时才配占一席之地；即便如此，也要配上正面目标，让 attention 落在该做的事上。

## Pruning

- 把每个意义放进**单一 source of truth**：一个权威位置，这样改变 behavior 是一次单点编辑。**Duplication**：同一意义出现在不止一处，耗费维护和 tokens，并把一个意义的 prominence 在阶梯上抬高到超出它真实 rank。（这是 leading word 的意外反面：leading word 是有意重复 token，从不重复意义。）
- **Environment** 也是 source of truth（`package.json` scripts、config files、directory layout、`--help` output），而 restate 它的文档是 **cache**：一份 lookup 的副本，只有在 lookup 昂贵时才赚得它的 load。Cache agent 无法通过查找找到的东西：未写下的 convention、选择背后的理由、没有 config 会承认的 gotcha。把 one-file、one-command 的 lookups 留给 environment，它们在那里不会过时。
- 逐行检查 **relevance**：它是否仍与文档所做的事相关？一行会因从不作用于任务（纯 exposition，或应该 disclosed 的 branch）而失去 relevance，或因它描述的行为或世界变化而过时。更短的文档更容易保持 relevant。没有 pruning discipline，默认命运是 **sediment**：层层沉淀的 stale layers，因为添加感觉安全、删除感觉冒险，直到你必须向下 core 穿过它们才能找到还活着的东西。
- 逐句猎杀 **no-ops**：一条 model 默认就会遵守的指令，花钱说了等于没说。测试（它相比 default 会改变 behavior 吗？）是 model-relative 的，不是 reader-relative 的：两个对 no-op 意见不一的人，是在对 default 意见不一，靠运行文档来解决，不是靠辩论。当一句失败时，删除整句，而不是从中修剪词。这个测试也给 leading words 打分：一个弱到打不赢 default 的词（_be thorough_，当 agent 已经差不多 thorough 时）是 no-op，解药是更强的词（_relentless_），而不是不同的技巧。
