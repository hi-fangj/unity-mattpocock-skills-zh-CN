---
name: retro
description: "对一次 coding session 做复盘。"
disable-model-invocation: true
---

用户要求做一次 **retrospective**。你要为改进 coding agent 的 **environment** 提出建议，让未来的 run 表现更好。

## Steps

1. 调用 Skill tool 运行 `writing-for-agents`，获取写作风格指南。

2. 阅读用户指定 session 的 primary sources。这可能需要在这台机器上搜索 session logs。如果用户没有指定 session，默认当前 session。

3. 在以下类别中寻找改进候选。

- **Navigation**：agent 找到正确文件的难易程度如何？文件之间是否存在隐藏依赖？**navigation pointer** 会不会有帮助？_Use when_ session 花了很长时间才找到某条信息。
- **Automated checks**：是否存在能抓住 agent 错误的 automated checks？linting、类型检查、测试、filesystem linters？_Use when_ agent 犯了一个本可以被 automated check 抓住的错误。
- **Coding standards**：是否应该给 **reviewer agent** 增加一条要执行的新规则？是否应删除或澄清某条现有规则？_Use when_ reviewer agent 未能抓住某个错误。
- **Global AGENTS.md**：是否有 steering 指令应该改放到 coding standards（或 automated checks）里？_Use when_ AGENTS.md 文件过大，无论在 repo 还是用户全局 scope。
- **Tool economy**：agent 是否做了本可精简的昂贵 tool calls？是否有特别 token 低效的自定义工具（CLI、MCP）？_Use when_ agent 做了一次昂贵的 tool call。
- **No-ops**：找出 steering 文件中不会改变 agent 行为的指令。_Use when_ steering 文件又大又难管理。
- **Information access**：寻找增加 agent 信息获取机会的途径。例如 tee 开发服务器日志、对第三方服务的只读访问。_Use when_ 某条关键信息对 agent 不可用。

4. 按严重程度排序，把这些候选呈现给用户。

## Reference

### Implementation vs Review

记住所有工作都经过两个阶段：implementation 和 review。implementation agent 承受最大的 **context pressure**：它负责探索、写代码和调试失败。

review agent 的 context pressure 最小：它拿到的是一个 diff，不需要探索，通常也不需要写代码或调试。

这意味着应该由 review agent 负责执行 coding standards，而不是 implementation agent。

### Files

你可以访问 repo 中的几类文件：

- `CLAUDE.md`/`AGENTS.md`：这些文件会被推入在这个 repo 中工作的任何 agent 的 context window。应当极其节制地使用，通常只放指向其他文件的 **navigation pointers**。
- `CODING_STANDARDS.md`：这个文件在 review 阶段读取，而不是 implementation 阶段。如果 standards 文件超过 1,000 行，就加入指向 docs 文件夹的 **navigation pointers**。
- Docs：把 docs 当作参考文件，由其他文件指向。写新 docs 之前先找已有的。
- Skills：docs 类内容用 skills 承载（它们的 description 会进入 agent 的 context window），user-invoked 命令也用 skills。遵循 `writing-for-agents` skill 的建议。
