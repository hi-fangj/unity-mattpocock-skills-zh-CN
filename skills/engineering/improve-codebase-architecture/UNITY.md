# Unity Architecture Review

在共享 deep-module vocabulary 之外应用这些检查。Repository instructions 与 `docs/agents/unity-development.md` 定义真实 layer direction 与 verification commands。

## Preserve real boundaries

- 把 `.asmdef` references 与 repository layer rules 当作 hard dependency evidence。Proposal 必须保留或明确修改该 graph，而不是移动 files 直到 compile。
- Generated output 归 generator 或 source configuration 所有。Generated wrappers 本身是 ownership boundaries，不是 refactoring candidates。
- Prefabs、scenes、serialized fields、events、resources 与 editor registrations 属于 module effective interface 时，把它们包括进来。

## Judge framework-facing shapes fairly

- 当 behavior 按 framework convention 有意拆分时，把 Entity 及其 Systems 作为一个 candidate module 评估。
- 当 `MonoBehaviour`、FairyGUI panel adapter、Unity callback、importer 或 editor menu 把 framework lifecycle 翻译到 deeper module 时，把它当作 adapter。Thinness 本身不是 architectural friction。
- 应用 deletion test 时，询问 Unity lifecycle、serialization、registration 与 cleanup obligations 会移到哪里。只有删除 candidate 会把 meaningful policy 分散到 callers 时，才建议 deepening。
- 区分 framework callbacks 中隐藏的 business policy 与必要 framework glue。把 policy 移向 owning domain/presentation module，同时保持 adapter explicit。

## Prefer real seams

- 不要只为创建 mocks 而提出 C# interfaces 或 dependency wrappers。要求真实 runtime、editor、platform、transport、asset-provider，或明确在 scope 中的 test adapter。
- 比较 interfaces 时尊重 main-thread、frame-order、scene-loading 与 serialization constraints。Constructor injection 对 Unity-owned objects 并非天然更好。
- Pure gameplay decisions 保留在 repository domain layer，Unity lifecycle concerns 保留在 Unity boundary。

## Make recommendations verifiable

对每个 candidate，指出最小受影响 assembly builds 与最近 faithful behavior check。对 deterministic gameplay、networking、loading 或 state/view synchronization changes，配对 logic-side 与 presentation-side evidence。依赖未证实 asset、scene 或 Editor assumptions 的 recommendation 标记为 speculative。
