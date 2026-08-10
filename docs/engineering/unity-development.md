Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=unity-development
```

```bash
npx skills update unity-development
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/unity-development)

## What it does

`unity-development` 把通用 implementation、bug-fixing、refactoring 与 test-first workflow 调整到 Unity 的真实边界。它不会假定每次变更都有快速 unit-test seam，也不会把一次成功的 C# compile 当作 scene、prefab、asset、UI 或 runtime behavior 已验证。

它先识别受影响 assembly 与 ownership，再从 pure C# test、EditMode、PlayMode、generated `.csproj` build、Editor operation、replay、scene/prefab smoke check 或 targeted manual flow 中选择最窄的 faithful evidence。

## When to reach for it

修改 Unity C#、`.asmdef`、`MonoBehaviour`、scene、prefab、Editor tool、FairyGUI/UI、asset loading、networking、generated code 或 deterministic/lockstep gameplay 时使用。用户要求 TDD、但项目没有稳定 test harness 时也应使用它来选择最近的 repeatable loop。

`ProjectSettings/ProjectVersion.txt` 是 setup 与其他 skills 识别 Unity repository 的 authoritative signal。先运行 [setup-matt-pocock-skills](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/setup-matt-pocock-skills)，让 repository-specific assembly、command 与 high-risk path 写入 `docs/agents/unity-development.md`。

## It's working if

- 每个变更都说明 affected assembly/layer 与 generated-code ownership。
- 编译检查与 runtime/Editor evidence 匹配真实 Unity boundary。
- Tests 只在已有 stable seam 能观察 behavior 时使用；否则采用 bounded、repeatable 的最近场景。
- 完成报告列出 checks、results、剩余 manual validation，以及 serialized/generated diff review。

## Where it fits

在 Unity repository 中，它是 `/ask-matt` 与项目 `AGENTS.md` 下层的 execution reference，并限定 `/prototype`、`/diagnosing-bugs`、`/code-review`、`/improve-codebase-architecture` 与 `/resolving-merge-conflicts` 的 Unity branches。它替换通用 `/implement` 和 `/tdd` 的验证假设，但不改变这些 skills 的产品/流程职责。
