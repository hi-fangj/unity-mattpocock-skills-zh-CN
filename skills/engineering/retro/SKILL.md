---
name: retro
description: "对一次 coding session 做回顾。"
disable-model-invocation: true
---

用户要求做一次 **retrospective**。你要对 coding agent 的**环境**提出改进建议，让未来的运行更好。

## Steps

1. 调用 Skill tool 运行 `writing-for-agents`，获取写作风格指南。

2. 阅读用户指定 session 的 primary sources。这可能需要在机器上搜索 session logs。用户没有指定 session 时，默认当前 session。

3. 在以下类别中寻找改进候选项。

- **Navigation**：agent 找到正确文件有多容易？文件之间是否存在隐藏依赖？一个 **navigation pointer** 会不会更省事？_Use when_ session 花了很长时间才找到某条信息。
- **Automated checks**：是否有 automated checks 能抓住 agent 犯的错误？Linting、typing、tests、filesystem linters？先读 repo 自己的 check command（它的 `package.json`/build tool `lint`/`check` scripts、它的 CI workflow），这样“check 已存在但没有接线或静默失效”本身就是 finding，而不是重新发明一个。一个没有任何 **guardrail** 的 repo（没有 pre-commit hook，也没有跑 lint/typecheck/test 的 CI job）本身就是一个 finding：没有 lint 的 repo 是一个持续的错失机会，不是中性默认。_Use when_ agent 犯了一个 automated check 本可抓住的错误，或 repo 根本没有 guardrail。
- **Coding standards**：应该给 **reviewer agent** 一条新规则去强制执行吗？应该删除或澄清既有规则吗？先对违规分类：**mechanical** 违规（固定的句法 pattern、被禁 API、import 形状、文件位置规则）一律交给 deterministic check：repo 自己 linter 里的 custom rule、新的 pre-commit hook 或新的 CI job，取 repo 语言和既有 guardrail 下最便宜的那个。默认建 check，而不是写规则。`CODING_STANDARDS.md` 只留给真正的 **judgement calls**（跨文件一致性、“与周围风格匹配”、任何 guardrail 都替代不了的东西）。_Use when_ reviewer agent 没能抓住某个错误。
- **Global AGENTS.md**：是否有 steering instructions 应该移到 coding standards（或 automated checks）里？_Use when_ AGENTS.md 文件过大，无论在 repo 还是用户全局 scope。
- **Tool economy**：agent 是否做了可以精简的昂贵 tool calls？是否有 token 效率特别低的自定义工具（CLI、MCP）？_Use when_ agent 做了昂贵的 tool call。
- **No-ops**：在 steering files 中寻找不会改变 agent 行为的 instructions。_Use when_ steering files 又大又难管理。
- **Information access**：寻找提高 agent 信息获取能力的机会。Tee dev server logs、对第三方服务的只读访问。_Use when_ 关键信息对 agent 不可得。

4. 按严重程度排序，把这些候选项呈现给用户。

## Reference

### Implementation vs Review

所有工作都经过两个阶段：implementation 和 review。Implementation agent 的 **context pressure** 最大：负责探索、写代码、调试失败。

Review agent 的 context pressure 最小：它拿到的是 diff，无需探索，通常也不用写代码或调试。

因此，coding standards 应由 review agent 负责强制执行，而不是 implementation agent。

### Files

你可以访问 repo 中的几类文件：

- `CLAUDE.md`/`AGENTS.md`：这些文件会被推进任何在此 repo 工作的 agent 的 context window。应极度节制地使用，通常只放指向其他文件的 **navigation pointers**。
- `CODING_STANDARDS.md`：这个文件在 review 时读取，不在 implementation 时读取。standards 文件超过 1,000 行时，向 docs folders 添加 **navigation pointers**。
- Docs：docs 作为 reference files 使用，由其他文件指向。写新 docs 前先找已有 docs。
- Skills：skills 用于 docs（因为它们的 description 会进入 agent 的 context window），或作为 user-invoked commands。遵循 `writing-for-agents` skill 中的建议。
