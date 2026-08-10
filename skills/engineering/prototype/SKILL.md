---
name: prototype
description: 构建一次性原型来回答一个设计问题。适用于用户想验证某个 state model 或 logic 是否感觉对，或探索 UI 应该长什么样时。
---

# Prototype

Prototype 是**用来回答一个问题的 throwaway code**。问题决定形状。

## Pick a branch

先识别正在回答哪个问题：来自用户 prompt、周围代码，或在用户在线时直接询问：

- **"Does this logic / state model feel right?"** → [LOGIC.md](LOGIC.md)。构建一个很小的交互式 terminal app，推动 state machine 跑过纸面上难以推理的 cases。
- **"What should this look like?"** → [UI.md](UI.md)。在单一路由上生成几种差异很大的 UI variations，并通过 URL search param 和浮动底栏切换。

当 `ProjectSettings/ProjectVersion.txt` 表明这是 Unity project 时，在选择 logic/UI branch 后读取 [UNITY.md](UNITY.md)。它替换 browser-specific artifact 与 run instructions，同时保留该 branch 的问题和 prototype discipline。

这两个分支会产出非常不同的 artifacts；选错会浪费整个 prototype。如果问题确实模糊且用户不可达，默认选择更匹配周围代码的分支（backend module → logic；page 或 component → UI），并在 prototype 顶部说明假设。

## Rules that apply to both

1. **从第一天就是 throwaway，并明确标记。** Prototype code 要靠近它实际会被使用的位置（放在被 prototype 的 module 或 page 旁边），这样上下文清楚；但命名要让随手读代码的人看出它是 prototype，不是 production。对 throwaway UI routes，遵守项目现有 routing convention；不要发明新的顶层结构。
2. **运行方式必须直接。** 使用一个 documented command、file、scene、prefab、editor menu 或 debug entrypoint。用户不应重建 setup 或 hidden state。
3. **默认不持久化。** State 保存在内存中。Persistence 是 prototype 要_检查_的东西，不该成为依赖。如果问题明确涉及 database，就用 scratch DB 或带有清晰 “PROTOTYPE — wipe me” 名称的本地文件。
4. **跳过 polish。** 不写 tests，不做超过“能跑起来”所需的 error handling，不做 abstractions。重点是快速学到东西。
5. **暴露 state。** 每次 action（logic）或每次 variant switch（UI）后，打印或渲染完整相关 state，让用户看到发生了什么变化。
6. **完成后 capture。** 把验证过的 decision 折进真实 work，并记录 question、verdict 与 evidence。只有用户或 repository workflow 要求保留 artifact 时，才把 prototype 作为 primary source 放在 throwaway branch；否则在 capture decision 后移除它。
