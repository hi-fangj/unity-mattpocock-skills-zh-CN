Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=pre-implementation-conflict-check
```

## What it does

`pre-implementation-conflict-check` 是需求确认完成后的实施前闸门。它将需求确认文档与 `AGENTS.md`、`CLAUDE.md`、`CONTEXT.md`、`CONTEXT-MAP.md`、ADR、代码规范、架构规范、测试规范和其他项目约束进行对照，列出冲突并用选项让用户逐项裁决。

它也会识别疑似过期或互相污染的文档。关键冲突没有解决前，它不会进入代码生成或功能实现；全部通过后才输出明确的“可以开始实现”结论。没有具体需求时，也可以用它做定期文档审计，整理多目录 `CONTEXT.md`、ADR 和项目约束。
