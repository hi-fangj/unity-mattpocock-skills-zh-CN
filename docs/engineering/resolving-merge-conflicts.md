Quickstart:

```bash
npx skills add hi-fangj/unity-mattpocock-skills-zh-CN --skill=resolving-merge-conflicts
```

```bash
npx skills update resolving-merge-conflicts
```

[Source](https://github.com/hi-fangj/unity-mattpocock-skills-zh-CN/tree/main/skills/engineering/resolving-merge-conflicts)

## What it does

`resolving-merge-conflicts` 逐个 hunk 地处理正在进行的 git merge 或 rebase conflict，并完成整个操作——解决、检查、然后提交。

它按 **intent** 来解决，而不是按文本。在触碰一个 hunk 之前，它会把每一侧追溯回其 **primary source**——commit message、PR、原始 issue——以理解这个改动为什么被做出，然后在两者兼容的地方同时保留双方的 intent。它绝不发明新的行为来粉饰冲突，也绝不伸手去用 `--abort`：merge 总是要被完成的。

## When to reach for it

输入 `/resolving-merge-conflicts`，或者当任务合适时由 agent 自动触发。

当你正处于 merge 或 rebase 中途、而 git 在它自己无法解决的 conflict 上停下来时，使用它。它针对的是你眼前的这个 conflict——不是用来规划 merge，也不是用来调试事后坏掉的行为。如果 merge 已经完成，但现在有东西因为看不出的原因在失败，那就改用 [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs)。

## Resolving by intent

conflict 中的陷阱是把它当成文本问题——为了让冲突标记消失而选择 "ours" 或 "theirs"。这个 skill 把它当成一个 **intent** 问题。一个 hunk 的每一侧之所以存在，都是因为有人想要某样东西；解决方案必须在能做到的地方尊重双方的诉求，而在它们确实互不相容的地方，选择与 merge 所声明目标相符的那一个，并明确说出其中的取舍。

这正是 primary sources 之所以重要的原因。你无法保留一个你还没读过的 intent，所以这项工作从历史开始——commits、PRs、issue——而不是从 diff 开始。

## It's working if

- 每个被解决的 hunk 都保留了双方的行为，或者在无法做到时点明了取舍。
- 没有出现任何两个 branch 上都不存在的新行为。
- 项目自己的检查——typecheck、tests、format——被找到，并在提交前跑出绿色。
- merge 或 rebase 被一路推进到一个完成的 commit，绝不 abort。

## Where it fits

一个可随时调用的 standalone：你在 merge 或 rebase 卡住的那一刻调用它，它交还给你一棵干净、已提交的树。它天然的邻居是 [diagnosing-bugs](https://aihero.dev/skills-diagnosing-bugs)，因为一个干净解决却在事后行为异常的 merge 是一个诊断问题，而不是 conflict 问题。当你不确定哪个 skill 合适时，[ask-matt](https://aihero.dev/skills-ask-matt) 会为你路由。
