# 通过 harness 原生 question tool 进行 grilling

Grilling sessions（`/grilling`、`/grill-me`、`/grill-with-docs`，以及其他 skills 内部的 grilling）以普通聊天文本提问。把提问改道到 harness 内置 question UI（Claude Code 的 `AskUserQuestion`、宿主的 "question tool"、结构化或批量 questionnaire 模式）的请求不在范围内。

## 为什么这不在范围内

这些 skills 是 harness 中立的文本。它们运行在 Claude Code、Codex、Copilot、Cursor 以及 skills.sh 能安装进的任何环境；每个 harness 的 question tool 都不同，或者根本没有。在 skill 里点名某个 harness 的 tool，要么导致每个 grilling skill 里出现 harness 特定分支，要么让 skill 只在一个地方正常工作。

Question UI 也会改变 grilling 本身。结构化选择器会把模型推向带预设答案的多选题，这与 grilling 的目的正相反：grilling 要的是你用自己的话回答的开放问题，一次只走 decision tree 的一个分支。作者试过 `AskUserQuestion` UI，不希望它进入这些 skills。

如果你更喜欢自己 harness 的 question tool，在你自己的 `CLAUDE.md` / `AGENTS.md` 中说明（"grilling 时通过 AskUserQuestion 提问"）。这是一行式的个人偏好，不需要共享 skill 知道它。

## Prior requests

- #19: "grill-me: not always using the question tool"
- #643: "Proposal: Use AskQuestion for structured input during grilling sessions"
- #840: "Use host's question tool for grilling sessions"
- #1107: "Grilling doesn't use Agent's native QA tool OOTB"
- #1152: "Proposal: experimental HTML questionnaire mode for batch grilling"
