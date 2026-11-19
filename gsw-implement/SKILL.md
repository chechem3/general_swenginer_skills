---
name: gsw-implement
description: "USE WHEN a person wants an agent to implement a Draft plan or a class S small item. Triggers: 'implement', 'start coding', 'do it', '实现', '开发', '开干', '动手'. Checks that the plan has a finished design (status Draft), creates the feature branch from the default branch, and works test-first inside the plan's Decision. Never changes doc/plans/."
---

# gsw-implement

生命周期：工作于 `Draft` 计划（计划状态不变，`Done` 由 gsw-ship 标记）。

## 步骤

1. 找到工作：这个人给的 slug，或列出 `status: Draft` 的计划让他选；
   class S 小项：一个跟踪项 ID + 一句话目标，无计划文件。
2. 读计划（`doc/plans/records/<file>`，本地落后先 pull）。状态必须是 `Draft`；
   还是 `Idea`（设计没写完）就停下："先用 gsw-plan 把 Decision、验收检查和步骤写好"。
   class S 跳过这一步。
3. 从最新的默认分支切工作分支：
   `git switch -c <type>/<tracker>-<slug-words-with-dash>`（tracker 无则 `pending`；
   字符限 `a-z`、`0-9`、`-`）。`<type>` 按工作性质选：`feature/` 新功能或新文档；
   `bugfix/` 修错误行为或错误文档内容；`refactor/` 重构、清理、性能、文档重组。
   只为某个发布修的从 `release/vX.Y` 切出。已有该分支就接着用。
4. 读 `## Decision`、`## Acceptance checks`、`## Steps`，把工作拆进 harness 的
   todo 列表；实施细节不写进计划文件。你写的文本用计划的 `language`。
5. 测试先行：先写验收检查或一个会失败的测试，再写实现。
6. 小步提交，用这个人的 git 身份，trailers：`Plan: <slug>` 与 `Item: <tracker>`。
7. 若 Decision 必须改：停下，让这个人用 gsw-plan 修改计划，再回来继续。
8. 验收检查全过后说："下一步：gsw-ship"。

## 不要

- 不做 Decision 之外的工作；不改计划未点名的契约、阈值、外部行为。
- 功能分支不改 `doc/plans/` 下任何文件。
- 不合并、不打 tag、不推默认分支。
