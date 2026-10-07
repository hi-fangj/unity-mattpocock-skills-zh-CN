# 为避开 harness 内置命令而重命名 skills

当某个 harness 自带了与现有 skill 同名的内置命令或 skill（`/prototype`、`/research`、`/code-review` 等）时，不会因此重命名 skill。为避免冲突而重命名 skill 或添加 alias 的请求不在范围内。

## 为什么这不在范围内

Harness 不断新增内置命令，而且每个 harness 自己起名。每次有 harness 撞上我们已有的名字就重命名一次，意味着要在 Claude Code、Copilot、Devin 等环境间追逐一个移动的靶子，并且每次都打断所有用户的肌肉记忆和 skills 之间的交叉引用。

Harness 如何解析两个同名的东西，是 harness 的行为，不是 skill 的。无论怎么解析，skill 文本本身都是正确的。

出路是 namespaced invocation。Claude Code plugin 会把每个 skill 挂在 plugin 名下，所以你总是可以显式调用我们的 skill：

```
/mattpocock-skills:research
/mattpocock-skills:code-review
```

如果你通过 skills.sh 安装，文件归你所有：重命名 folder 和你副本中的 `name:` 字段即可。

如果某个 harness 完全没有提供访问 namespaced 或重命名 skill 的途径，请向该 harness 反馈。

## Prior requests

- #423: "/code-review shadows Claude Code own review skill"
- #483: "/code-review name clash with Claude Code built-in"
- #857: "Rename the research skill to avoid conflict with Copilot's built-in /research command"
- #1019: "Shorthand `/prototype` conflicts with hidden new Claude Code built-in skill `/prototype`"
- #1037: "Skill name overlap with devin cli inbuilt commands"
