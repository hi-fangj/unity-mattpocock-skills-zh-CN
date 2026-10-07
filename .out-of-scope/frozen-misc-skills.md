# 对 `misc/` skills 的改动

`skills/misc/` 下的 skills（`git-guardrails-claude-code`、`migrate-to-shoehorn`、`scaffold-exercises`、`setup-pre-commit`，以及其他落在那里的任何 skills）处于冻结状态。针对它们的 bug reports、feature requests 和 PRs 一律关闭。

## 为什么这不在范围内

`misc/` 是作者保留但很少使用的 skills 的去处。它们不被推广：不进入 Claude Code plugin，没有 docs page，作者也不在日常运行它们，因此既无法判断一个修复是否正确，也无法察觉某个修复何时破坏了其他东西。以与推广 skills 相同的标准维护它们，成本一样高，收益却只有零头。

它们仍留在 repo 中、不受维护，因为对某些人来说它们原样可用。如果某个 skill 对你不可用，用 skills.sh 安装并修改你自己的副本：那些文件归你编辑。

如果某个 `misc/` skill 之后被重新提升回 `engineering/` 或 `productivity/`，从那一刻起它重新回到范围内。

## Prior requests

- #14: "Potential typo in setup-pre-commit/SKILL.md"
- #301: "`block-dangerous-git.sh` is bypassed by any git global option (e.g. `git -C <dir> push`)"
- #465: "git-guardrails-claude-code documentation is missing benefits over built in Claude code deny rules"
- #898: "git-guardrails-claude-code: hook fails open without jq and misses common git spellings"
