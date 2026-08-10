---
name: resolving-merge-conflicts
description: "适用于需要解决正在进行的 git merge/rebase 冲突时。"
---

1. **查看当前状态。** 确认 Git 正在 merge、rebase、cherry-pick 还是 revert。触碰任何文件前，读取 conflict list、current history、repository instructions 与已有 user changes。

2. **寻找 primary sources。** 把每一侧追溯到 commit、issue、spec、generator、configuration 或 asset authoring source。按 intent 解决，不要只处理 conflict markers。

3. **适用时分类 Unity files。** 读取 repository Unity development document，然后按 ownership 处理 conflicts：
   - C# 与 structured config：在最小 coherent behavior 上合并双方 intent。
   - `.meta`：保留 surviving asset identity 对应的 GUID；不要组合 GUID，也不要意外创建 replacement identity。
   - Scene、prefab 与其他 Unity YAML：优先使用 UnityYAMLMerge，或选择 coherent asset version 后在 Unity 中重放另一侧 intent。只有理解 object identity 与 serialized references 时才 hand-merge。
   - Binary assets：选择一个 primary source；双方都需要时，在 owning authoring tool 中重新应用另一侧 change。
   - Generated code：解决 generator 或 source configuration 后 regenerate，不要手写 generated result。

4. **解决每个 conflict。** 兼容时保留双方 intent；冲突时选择符合本次 operation 目标的一方并记录 trade-off。保留 unrelated worktree changes。如果 abort 是最安全的正确结果或用户要求 abort，先报告 evidence 并请求确认。

5. **验证 resolved surface。** 运行受影响 assemblies 与 behaviors 的 repository-specific checks。Unity 中检查 `.meta` 与 serialized diffs、编译受影响 `.csproj`，并运行 conflict 所需的最窄 scene、prefab、PlayMode、generator、resource 或 lockstep check。

6. **只推进用户请求的 operation。** 仅 stage 已解决 conflict 的 files，保持 unrelated user changes 与 staging state 不变。用户请求包含完成 operation 时才继续 merge/rebase；只有 Git 为完成该 operation 所需或用户明确要求时才 commit。
