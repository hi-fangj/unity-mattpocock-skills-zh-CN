# Productivity

通用工作流工具，不限于代码。

## User-invoked

只有在你显式输入名称时才能调用（Claude Code：`disable-model-invocation: true`；Codex：`agents/openai.yaml` 中的 `policy.allow_implicit_invocation: false`）。

- **[grill-me](./grill-me/SKILL.md)**：围绕计划或设计进行持续追问，直到 decision tree 的每个分支都被解决。
- **[handoff](./handoff/SKILL.md)**：把当前对话压缩成 handoff document，让另一个 agent 可以继续。
- **[teach](./teach/SKILL.md)**：使用当前目录作为 stateful teaching workspace，在多个 sessions 中教用户一个新 skill 或概念。
- **[to-questionnaire](./to-questionnaire/SKILL.md)**：把你无法独自回答的 decision 转成一份 Markdown questionnaire，交给唯一能回答的那个人，异步填写，或在会议中一起填写。
- **[wait-what](./wait-what/SKILL.md)**：当一条消息没有落地时触发它。agent 会用你缺失的 context、用平实的语言、用你的 `CONTEXT.md` 词汇重新表述。
- **[writing-for-agents](./writing-for-agents/SKILL.md)**：编写供 agents 消费的文档：skills、AGENTS.md/CLAUDE.md，以及任何 agent 通过 pointer 到达的 doc。

## Model-invoked

模型或用户都可以调用（description 包含足够丰富的触发措辞，方便模型自动找到它们）。

- **[grilling](./grilling/SKILL.md)**：围绕计划、decision 或 idea 按轮次持续访谈用户，直到 design tree 的每个分支都被解决。
