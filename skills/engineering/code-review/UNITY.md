# Unity Review Checklist

只应用 review surface 实际触及的项目。Repository instructions 覆盖此通用 checklist。

- **Architecture:** 保持 asmdef 与 layer dependency direction；gameplay rules 不进入 presentation，Unity lifecycle code 不进入 pure domain layers。
- **Generated ownership:** 把 generated changes 追溯到 generator 或 source configuration。标记手工修改 output 的情况。
- **Assets and identity:** 连同 `.meta` files 检查新增、删除、重命名或移动的 assets。标记 missing metas、意外 GUID changes、broken references 和 accidental duplicate identities。
- **Serialized assets:** 检查 scene、prefab、ScriptableObject、material 与 importer YAML 中的 unintended churn、missing references、override loss 或 unrelated object changes。
- **UI lifecycle:** 验证 event registration/cleanup、重复 open/close、async cancellation，以及 generated UI wrappers 之外的 business logic placement。
- **Runtime lifecycle:** 检查 object ownership、disposal、Unity callback ordering、coroutine/task lifetime、scene unload、pooling 与 serialized-field compatibility。
- **Loading and builds:** 当相关路径变化时，检查 Loader、HybridCLR、asset package、manifest、version、code mode 和 platform implications。
- **Networking:** 检查 send/receive contracts、dispatch、disconnect、reconnect、timeout 与 version compatibility。
- **Determinism:** 对 Battle、LockStep 或 `LS*`，检查 identical-input logic consistency 与对应 ModelView consumption。
- **Verification:** 将已执行检查与受影响 Unity boundary 对照。Runtime、asset、scene、UI 或 platform behavior 不能只靠 compiler check。

把 review surface 之外已有的 unrelated warnings 与 serialized churn 当作 context，而不是本次 change findings。只报告 modified files 引入的新 warnings 或 churn。
