# Model-invoked 与 user-invoked

本仓库中的每个 `SKILL.md` 都是一个 skill。它们只按一个维度区分：**invocation**，也就是谁能调用它：

- **User-invoked**：只能由人类输入名称来调用。在 frontmatter 中设置 `disable-model-invocation: true`（Claude Code），并在 `agents/openai.yaml` 中设置 `policy.allow_implicit_invocation: false`（Codex）。`description` 面向人类：浏览 slash commands 时看到的一行摘要。去掉触发词列表（例如 "Use when the user says..."）。
- **Model-invoked**：模型和用户都可以调用。默认就是这种形式：省略 `disable-model-invocation`，并省略 `agents/openai.yaml` 中的 `policy` block。`description` 面向模型，并保留丰富的触发措辞（例如 "Use when the user wants..."、"mentions..."、"asks for..."），让自动调用能命中。判断一个 skill 是否应保持 model-invoked 的测试是：模型能否有意义地自行想到要用它？（复用是抽出 skill 的理由，不是这个判断标准。）

每个 agent harness 都用自己的方式把 user-invoked skill 排除在模型可调用范围之外，因此只有人类能触发它，其他 skill 也不能调用它。User-invoked skill 可以调用 model-invoked skills，但永远不能调用另一个 user-invoked skill。

每个 skill 的 `SKILL.md` 旁都必须有 `agents/openai.yaml`。它保存 Codex UI metadata：skill picker 使用的 `interface.display_name` 与 `interface.short_description`；对于 user-invoked skills，还保存与 `disable-model-invocation` 配对的 `policy.allow_implicit_invocation: false`。两边必须保持同步：一个 skill 要么在两个 harness 中都是 user-invoked，要么都不是。

Bucket `README.md` 和顶层 `README.md` 都按 **User-invoked** 与 **Model-invoked** 对条目分组。

## 它们之间的依赖

依赖用**显式调用 Skill tool** 的指令表达（`Call the Skill tool with "grilling"`），而不是深层 `../other-skill/FILE.md` 交叉引用，也不是留给模型自行解读的裸 `/skill` 式提及。点名工具才是让它被触发的方式：多数 harness 把 skill 调用暴露为模型调用的一个 tool，把它写明比在 prose 里丢一个 `/name` 并期望它被当作命令的命中率更高。去掉前导 `/` 也让这条指令保持 harness 中立：单独的 skill 名称不携带任何对特定 harness 触发语法的假设。共享 reference docs 存放在拥有它们的 skill 内；其他 skills 通过调用 Skill tool 运行该 skill 触达这些资料，而不是跨目录链接。

这条约定针对的是**可执行的**指令：一个 skill 自己的步骤让 agent 现在就去运行另一个 skill。仅为人类罗列候选 skills 的 router prose（`ask-matt`、各 bucket `README.md`）没有调用任何东西，因此其中的 `/skill` 式名称保留为普通标签。

Skill tool 每次只接受一个 skill。需要两个 skills 的步骤就是两次调用，而不是一次调用带两个名称：写成（`Call the Skill tool twice, for "grilling" and "domain-modeling"`），不要写成 "call it with X and Y"，后者读起来像一次调用同时接受两者。

整条约定只在被点名的 skill 是 **model-invoked** 时成立。User-invoked skill 永远无法通过这种方式触达，绝对不行：按上文的不变量，其他 skill 不能调用它，包括通过向 Skill tool 点名的方式。当某个步骤的前置条件是一个 user-invoked skill（例如 `setup-matt-pocock-skills`）时，把它表述为交给人类执行的指示："tell the user to run `/setup-matt-pocock-skills`"，绝不表述为 Skill tool 调用。

## 被动与主动的 domain 工作

只是为了词汇而 _读取_ `CONTEXT.md`，是一条普通 prose pointer，不是 `domain-modeling` skill。只有主动构建和打磨 domain model 的纪律（挑战术语、构造 edge-case scenarios、写 ADRs、内联更新 `CONTEXT.md`）才是 `domain-modeling`。
