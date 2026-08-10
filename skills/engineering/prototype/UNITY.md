# Unity Prototype

在 production gameplay flow 之外构建 prototype，并遵守 repository architecture 与 generated-code boundaries。优先使用 scratch scene、duplicated prefab、debug-only component、EditorWindow 或 temporary FairyGUI package，不要修改 shipping scene 或 generated wrapper。

## Logic branch

根据 audience 选择 artifact：

- Designers/domain experts 需要独立于 Unity 的 portable click-through model 时，使用现有 HTML logic demo。
- 问题依赖 project types、fixed-point math、Entity/System behavior、Unity serialization 或 battle inputs 时，使用 small C# harness、EditorWindow、debug command 或 isolated PlayMode scene。

每次 action 后展示完整相关 state。提供 happy path、awkward edge case 与 illegal action 的 guided scenarios。Gameplay decisions 保持在 Model-compatible code；harness 放在 production flow 之外。

## UI branch

使用真实 presentation technology 与代表性 data，创建两到三个结构明显不同的 variants：

- FairyGUI work：在 owning FairyGUI project 或 temporary package/component 中 prototype，再通过 disposable panel 或 debug entrypoint 运行。
- Unity scene/prefab UI：复制 owning asset 或使用 scratch scene。通过 debug selector、inspector enum 或 temporary controller 切换 variants。

在 target aspect ratios 与 safe areas 验证 layout。Capture screenshots 或 short recordings 供比较。除非 mutation behavior 本身就是问题，否则 stub real mutations。

## Cleanup

记录 winning decision 与理由。通过 normal implementation workflow 推进 decision，不要直接 shipping prototype code。除非用户要求在 separate branch 保留，否则移除 temporary scenes、prefabs、debug registrations、copied resources 与 prototype-only asset references。
