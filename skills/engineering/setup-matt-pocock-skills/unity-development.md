# Unity Development

将本文档与 `$unity-development` 配合使用。用 repository facts 替换通用 examples，同时保留已有 project-specific rules。

## Architecture and ownership

记录项目 assembly/layer direction、layers 之间允许的 dependencies，以及 gameplay、presentation、`MonoBehaviour`、editor 和 loading code 的归属。列出 generated directories，并说明拥有它们的 generator 或 source configuration。

## Feedback matrix

为每个受影响 assembly 或 layer 记录：

| Area | Smallest compiler check | Faithful behavior evidence |
| --- | --- | --- |
| Pure domain 或 gameplay logic | 存在 stable seam 与 runner 时使用现有 focused test；否则编译 generated project | Worked examples、replay 或最近 game flow |
| UI 或 presentation | 编译受影响 presentation project | Targeted PlayMode 或 manual open/interact/close/reopen flow，包括 event cleanup |
| `MonoBehaviour`、prefab 或 scene | 编译 owning runtime project | Serialized reference inspection 与 owning lifecycle smoke check |
| Editor tooling | 编译 owning Editor project | 对代表性 input 运行确切 menu、importer、generator 或 build operation |
| Loader、assets、networking、generated code、deterministic simulation | 编译每个受影响 project | 每个受影响侧都有明确 impact path 与 focused end-to-end scenario |

使用仓库的确切命令。Generated `.csproj` 可以是 compiler entrypoint，但不是 source configuration。记录何时必须由 Unity regenerate，避免暗示 agents 应直接编辑它。

## Development loop

只有现有 test infrastructure 能忠实观察行为时才使用 red-green。否则实现最小 observable slice，编译受影响 assembly，并运行最近 Unity boundary。TDD 请求不包含新增 test assembly 的权限，除非该 infrastructure 已明确在 scope 中。

Compiler output 的 success rule 是零 errors，且修改文件没有引入新 warnings。保留已有无关 warnings 作为 baseline，不把它们扩展成 cleanup work。

## Unity-only checks

记录哪些条件需要 EditMode、PlayMode、scene/prefab smoke checks、resource builds、player builds、platform checks 或 project-specific Editor commands。让 expensive checks 取决于变更行为，并说明 Editor ownership 或 environment prerequisite。

## High-risk paths

列出项目的 startup、asset/package、network/protocol、code-generation、deterministic simulation、save/migration 或 platform-sensitive paths。为每一项记录 impact path 与完成前所需的最窄证据。

## Completion report

要求 agents 报告受影响 layer、已运行 checks、compiler result、runtime/editor scenario 和剩余 human validation。检查 generated/serialized diffs、event cleanup、architecture boundaries 与 unrelated changes。只有用户要求时才 commit 或调用独立 review workflow。
