---
name: gsw-status
description: "USE WHEN a person asks for the state of plans, features, a module, a release, or a requirement. Triggers: 'status', 'what next', 'where are we', 'release notes', 'trace', '状态', '进度', '下一步', '发布说明', '追溯'. Read-only: answers from doc/plans/ and git; with no input, shows open plans with the next step for each. Never changes a file."
---

# gsw-status

只读。

## 按输入作答

| 输入 | 回答 |
|---|---|
| 无输入 | 1. 未完成的计划（`status: Idea`/`Draft`/`Locked`），各附下一步：Idea → gsw-plan 成稿；Draft → gsw-approve 评审锁定；Locked → gsw-implement 实现；有分支无 MR → gsw-ship。2. 本地 `doc/plans/` 落后远端（`git rev-list --count HEAD..origin/<default> -- doc/plans/` 大于 0）就提示先 pull。 |
| 模块名 | `doc/plans/records/` 里 `packages` 含该模块的计划，按状态分组；陈旧计划：`Draft`/`Locked` 30 天未更新、30 天无带其 `Plan:` trailer 的提交 |
| 计划 slug | 摘要、状态、MR/PR（front matter `mr`）、提交（`git log --all --grep "Plan: <slug>"`）、`## Result` 内容 |
| tag 范围 `vX..vY` | 发布说明草稿：`git log vX..vY` 里状态 `Done` 的计划与带 `Plan:` trailer 的提交，按模块分组 |
| 需求 ID | `requirements` 含该 ID 的计划，及这些计划的提交 |

## 跟踪系统

本机有对应 CLI 且已登录时（Jira / Feishu Projects / gh issue 等），比对每个计划
`tracker` 项的状态与计划状态，列出差异；否则写"未检查跟踪系统"。
`pending` 列为"暂无跟踪项"。

## 规则

- 用对话的语言回复；引用计划原文时用其 `language`。
- 优先用 front matter（title/summary/status/class/mr）摘要作答；只有 slug 问题才打开
  完整计划。
- 不做的想法不算"关闭"：留在 `Idea` 即可；要清掉就从 `doc/plans/INDEX.md` 删该行
  （那是写操作，不属于本技能，提示这个人自己删或用编辑会话做）。
- 永不改文件。回答前后 `git status` 必须不变。
