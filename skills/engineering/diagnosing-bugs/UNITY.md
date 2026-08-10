# Unity Bug Diagnosis

选择 loop 前，读取 repository `AGENTS.md`、Unity development document、受影响 `.asmdef` 与 Unity version。

## Feedback loops

选择能观察真实 symptom 的最窄 loop：

- **Model 或 deterministic battle logic:** 固定 inputs、battle seed、replay、command stream 或 known log comparison，加受影响 assembly build。
- **ModelView 或 FairyGUI:** 打开、交互、关闭并重新打开 target panel，同时捕获 state、callbacks 与 console output。
- **MonoBehaviour、scene、prefab、animation、physics、coroutine:** 在 owning scene/prefab 中运行 focused PlayMode scenario，并记录相关 callback 或 frame sequence。
- **Loader、HybridCLR、YooAsset:** 固定 package、version、manifest 与 code mode 后运行受影响 startup/play mode。
- **Device 或 platform issue:** Development build、确切 device/OS、Player.log、timestamped recording，以及 platform profiler/crash artifact。
- **Performance 或 memory:** Unity Profiler、Memory Profiler、Frame Debugger 或 repeatable frame-time capture。比较相同 scene、content、device、quality settings 与 input。

Compilation 只有对 compilation/code-generation failures 才构成 loop。Passing build 不能让 runtime symptom 变绿。

## Instrumentation

优先使用 breakpoints 与现有 diagnostic tools。Temporary logs 使用唯一 tag；ordering 重要时包含 entity/object identity 与 frame/tick，并保持 lockstep diagnostics deterministic。移除 temporary logs、debug objects、profiler markers、copied scenes 与不打算保留的 generated captures。

## Regression evidence

只有 automated test 能通过 stable seam 观察真实 failure 时才使用它。否则保留 minimal replay、input sequence、scene/prefab scenario、profiler capture 或 exact manual checklist 作为 regression artifact。对 Battle、LockStep 或 `LS*`，同时验证 deterministic logic 与 ModelView consumption。
