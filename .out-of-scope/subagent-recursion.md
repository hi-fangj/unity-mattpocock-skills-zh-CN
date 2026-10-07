# 防止 sub-agents 递归调用 skills

Skills 不会添加针对 sub-agents 递归调用 skills 或派生嵌套 agents 的防护（`/code-review` 的 sub-agent 再次调用 `/code-review`、`/research` 的 agent 再派生另一个 agent）。添加 leaf-agent instructions、深度限制或 "do not invoke this skill" 字样的请求不在范围内。

## 为什么这不在范围内

阻止无限 subagent 循环应该是 harness 的职责，而不是 skills 的。Harness 决定一个 sub-agent 能触达哪些 tools 和 skills、嵌套可以多深，所以在那里设置的限制对所有 skills 一次性生效。写进某个 skill prose 的防护只覆盖那个 skill，每次运行都消耗 tokens，而且仍然依赖模型选择服从。

如果 sub-agent 无限制地递归或扇出，请向 harness 反馈。

## Prior requests

- #530: "research skill: spawned background agent can recursively re-spawn another agent (uncontrolled nesting)"
- #573: "code-review sub-agents recursively invoke /code-review and fan out"（PR #1186，"code-review: make the Standards and Spec sub-agents leaf reviewers"，已关闭）
