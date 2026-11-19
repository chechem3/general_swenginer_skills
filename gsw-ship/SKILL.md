---
name: gsw-ship
description: "USE WHEN implemented work is ready for review and the person wants a merge request. Triggers: 'ship it', 'open MR', 'create merge request', '提MR', '提交合并', '发起合并请求'. Checks that doc/plans/ did not change, runs the tests, updates the affected docs, pushes, opens the MR/PR, then marks the plan Done with the MR link."
---

# gsw-ship

生命周期：开 MR/PR，并把计划标记为 `Done`。

## 步骤

1. 检查：当前在 `feature/`、`bugfix/` 或 `refactor/` 工作分支上，不在默认分支或
   `release/*` 上。
2. 检查分支不含对 `doc/plans/` 的改动
   （`git diff --name-only <default-branch>...HEAD -- doc/plans/` 必须为空）。不满足就停下；
   计划的改动走 gsw-plan，此时不开 MR/PR。
3. 跑改动涉及的测试，逐个独立进程跑（按本仓库的跑法：Makefile 目标 /
   `pytest` / `unittest discover`）。有失败就停下并报告。
4. 更新被这次改动弄错或弄缺的当前文档：package `README.md`、概念/架构/操作页。
   加一条指向计划文件（`doc/plans/records/YYYYMMDD_<slug>.md`）的"Why"链接。
5. 提交（这个人的 git 身份）：主题 = 需求 ID（如有）+ 简短祈使句；trailers：
   `Plan: <slug>`、`Item: <tracker>`；不加工具或 agent 署名。
6. 把 MR/PR 描述写到 `output/YYYYMMDD_HHMMSS_mr/description.md`（不进 git）：
   计划链接与状态、class、需求 ID、验收检查表及结果、改动的文档页。
   用计划的 `language`。
7. 推送并开 MR/PR（GitLab 用 `glab`，GitHub 用 `gh`；都没有就开网页把描述贴进去）：
   - `git push -u origin HEAD`
   - `glab mr create --source-branch "$(git branch --show-current)" \
     --target-branch <default> --title "[<tracker>] <标题>" \
     --description "$(cat <描述文件>)" --yes`
8. **标记完成**（class S 无计划则跳过）：编辑该计划文件，`status` 置 `Done`，
   front matter `mr` 填 MR/PR 链接，`## Result` 写一两行（MR/PR 号、日期、验收检查
   结果）。更新 `doc/plans/INDEX.md`。单独提交（提交只含 `doc/plans/`）并推送。
9. 回复：MR/PR 链接和评审人（有 CODEOWNERS 就列对应 owner），以及已标记
   `Done` 的计划路径。

## 不要

- 不合并。不推默认分支。不跳过失败的测试。
- 除第 8 步外不改 `doc/plans/` 下的文件。
