# Unity Module Design

用共享词汇描述 architecture roles，同时保留 Unity literal names。Repository instructions 与 `docs/agents/unity-development.md` 覆盖通用 examples。

## Establish the effective interface

对 Unity code，interface 不只包含 public C# methods，还可能包含：

- `.asmdef` dependency direction 与 platform/editor constraints
- serialized fields、prefab/scene ownership、inspector configuration 和 asset identity
- Unity callbacks、frame order、main-thread requirements、coroutines，以及 disposal/event cleanup
- Entity/Component data 与允许操作它们的 Systems
- generated-code ownership，以及修改它的 source configuration 或 generator

保留 `Component`、`MonoBehaviour`、`GameObject` 与 framework class names 等 literal identifiers。只有描述 architecture roles 时才使用 **module**、**interface**、**seam** 和 **adapter**。

## Choose dependency patterns at the owning boundary

- 把 deterministic gameplay policy 保留在 repository domain layer。Unity lifecycle、presentation、serialized references 与 asset access 放在拥有这些关注点的 layer。
- 不要把 constructor injection 强加给 Unity-created `MonoBehaviour` 或 serialized objects。优先使用 repository composition root、serialized reference、factory、Entity/System 或 explicit initialization convention；把 ordering 与 ownership 作为 interface 一部分。
- 当 `MonoBehaviour`、FairyGUI panel hook、event bridge、importer 或 editor callback 把 framework invocation 翻译到更深 module 时，把它视为 adapter；它可以有意保持 shallow。
- 当 framework convention 有意把 data 与 behavior 分开时，把 Entity 及其 Systems 作为一个 module 评估。按 caller-visible operation 判断 depth，而不是 class size。
- 让 `.asmdef` seams 与 repository dependency graph 对齐。会产生 reverse assembly reference 的 proposed interface，除非明确修改 architecture，否则不可行。

## Add seams only for real variation

只有至少存在两种有意义 behaviors 时才引入 C# interface 或 adapter，例如 runtime/editor、platform variants、transport implementations、asset providers，或已在 scope 中的 test infrastructure。仅为了 unit-test Unity callback 而创建的 mock，不构成 useful seam。

控制 time、randomness、network transport 与 asset services 等真实 nondeterminism。替换 internal collaborators 只会镜像 implementation structure 时，保持 concrete。

## Verify through faithful surfaces

- 存在 deterministic seam 时，通过它测试 pure calculations 与 state transitions。
- Serialization、callbacks、scene loading、prefabs、animation、physics、rendering 与 UI interaction 要在 EditMode、PlayMode、Editor 或 manual boundary 验证。
- Deepening module 时保留有价值 boundary tests；只有 new evidence 覆盖相同行为与 failure mode 时才替换 narrow test。
- 对 deterministic/lockstep gameplay，验证 identical logic inputs 与 presentation 对结果 state 的消费。

使用 Design It Twice 时，要求每个 proposal 在 C# methods 之外，同时说明 assembly placement、lifecycle/serialization contract、asset ownership、thread/frame assumptions 与 verification surface。
