# Unity Verification Selection

使用此 reference 选择能够忠实观察变更行为的最便宜证据。仓库指令覆盖此通用矩阵。

## Evidence matrix

| Change type | Primary feedback | Completion evidence |
| --- | --- | --- |
| Pure domain rule、calculation、state transition | 存在 public seam 时使用已有 unit 或 EditMode test；编译受影响 assembly | Worked examples 通过且受影响 assembly 编译成功 |
| 不依赖 frame 或 scene lifecycle 的 Unity API | 稳定时使用 EditMode test；编译受影响 assembly | 通过 public boundary 观察 serialized 或 API behavior |
| `MonoBehaviour`、coroutine、physics、animation、rendering | Targeted PlayMode check；编译受影响 assembly | 相关 lifecycle 或 frame sequence 行为正确 |
| UI panel 或 interaction | Pure decisions 使用 presenter/system test；targeted PlayMode 或 manual UI flow | Open、interact、close、reopen 和 cleanup 成功 |
| Prefab、scene、inspector、serialized field | Scene/prefab smoke check 加 serialized diff inspection | Reload 后 references 保持，目标 flow 可运行 |
| Editor tooling 或 importer | 可用时使用 Editor test，否则运行确切 menu/import operation | 对代表性 asset 操作成功且没有 collateral changes |
| Asset loading、bundles、addressables、YooAsset | Compile 加最窄 initialization/loading scenario | 目标 asset 在预期 play mode 或 build configuration 中解析成功 |
| Network 或 protocol path | 可用时使用 contract/dispatcher test，加 targeted connection scenario | 覆盖与变更相关的 send、receive、reconnect 和 failure behavior |
| Deterministic 或 lockstep simulation | Pure simulation test 或已知 replay/log comparison，加 view consistency check | 相同输入产生相同 logic result，且 view 正确消费结果 |
| Generated code | 修改 generator 或 source configuration 后 regenerate | Regeneration 可重复，且只有预期 generated output 改变 |

## Test-first eligibility

仅当以下条件全部成立时使用 test-first development：

- Assertion 通过稳定 boundary 观察 caller/player-visible behavior。
- 结果 deterministic，或 time、randomness、network、asset boundaries 可被干净控制。
- 仓库已有合适 test assembly 和 runner，或新增它已明确包含在 scope 中。
- Test cycle 足够快，可重复运行且不依赖偶然 Editor state。

以下情况占主导时，优先使用 compile-and-exercise slices：

- Correctness 依赖 frame order、Unity callbacks、scene loading、serialized references、animation、rendering 或 platform integration。
- Proposed test 主要 mock `GameObject`、`MonoBehaviour`、FairyGUI 或内部 collaborators。
- 创建 harness 会超出请求范围，或反而遮蔽正在修复的行为。

## Boundary rules

- 测试 public systems 和 domain operations，而不是 private helpers。
- 控制 time、randomness、network transport、file access 和 asset services 等真实 boundaries。可行时对内部 collaborators 使用真实 project objects。
- Expected values 应来自 specifications、worked examples、fixtures、known replays 或 serialized reference assets，不要从 implementation 复制。
- 当 gameplay state 与 presentation 必须一致时，配对 logic-side 与 view-side verification。

## Command hierarchy

1. 使用仓库记录的命令或现有 test runner。
2. 当 generated project 是可靠 compiler check 时，编译最小受影响项目。
3. 存在时运行 focused EditMode 或 PlayMode test target。
4. 只有项目支持、且没有冲突 Editor instance 占用项目时，才使用 Unity batch mode。
5. 自动化无法忠实观察行为时，使用 targeted manual check；无法运行时报告确切未检查场景。
