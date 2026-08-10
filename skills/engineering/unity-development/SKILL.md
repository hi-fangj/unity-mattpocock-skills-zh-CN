---
name: unity-development
description: 将实现、bug 修复、重构和 test-first 工作流适配到 Unity 项目。适用于修改 Unity C#、assembly definitions、MonoBehaviours、scenes、prefabs、editor tools、UI、asset loading、deterministic gameplay，或通用实现/TDD 工作流错误地假定存在常规命令行测试套件时。
---

# Unity Development

使用项目真实的 Unity 验证面。把 test-first development 当作一种可选 feedback loop，而不是通用前置条件。

## Ground the task

1. 读取仓库的 `AGENTS.md` 或 `CLAUDE.md`、相关 domain documentation，以及任何 `docs/agents/unity-development.md`。
2. 定位受影响的 assembly、Unity boundary、generated-code ownership 和现有 validation entrypoints。
3. 编辑前说明重要假设，并把请求转换为可观察的 success criteria。

选择 pure C# tests、EditMode tests、PlayMode tests、compilation、scene/prefab checks 和 manual validation 时，读取 [references/verification.md](references/verification.md)。如果仓库自己的 Unity development 文档提供了更精确的命令或约束，以它为准。

## Choose the feedback loop

选择测试前，先给每个变更行为分类：

- 当行为 deterministic、已有可观察 seam、test runner 可用，并且循环不依赖脆弱 Editor state 时，使用 red-green loop。
- 不满足这些条件时，使用最小可检查 vertical slice：明确预期行为，实现最小改动，编译受影响 assembly，再运行最近的 Unity-facing behavior。
- 用户明确要求 TDD、但仓库没有合适 harness 时，说明约束并使用最接近的可重复循环。TDD 请求本身不授权新增 test infrastructure；只有用户把该工作明确纳入 scope 时才添加。

把 gameplay rules 和 state transitions 保留在 domain code 中，使其以后能获得快速测试。Unity lifecycle、rendering、animation、prefab wiring 和 UI interaction 要在真实边界验证。

## Implement in slices

1. 一次完成一个行为闭环，并遵守仓库 architecture 与 code-generation boundaries。
2. 每个有意义 slice 后运行最便宜的相关 compile 或 test。
3. 只有当变更行为依赖 Unity Editor、PlayMode、assets、scenes 或 platform 时，才添加对应检查。
4. 对 startup、asset manifests、networking、code generation 或 deterministic simulation 等高风险系统，追踪 impact path 并验证所有受影响侧。

## Verify completion

- 优先使用仓库记录的命令。否则，当 generated `.csproj` 支持时，使用 `dotnet build <project>.csproj --no-restore --nologo --verbosity:minimal` 编译最小受影响项目。
- 要求零 compilation errors，且修改文件没有引入新 warnings。记录已有无关 warnings，不做宽泛清理。
- 只有变更风险或 acceptance criteria 要求时，才运行 full suites、player builds、resource builds 或 broad PlayMode passes。
- 报告每项已运行检查、结果，以及仍需 human 或已打开 Unity Editor 验证的行为。
- 最终检查 diff 的 architecture boundaries、generated files、serialized assets、event cleanup 和 API comments。只有用户要求时才 commit 或调用独立 review workflow。
