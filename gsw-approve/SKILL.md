---
name: gsw-approve
description: "USE WHEN a Draft plan has been reviewed and the person approves it for implementation. Triggers: 'approve the plan', 'lock the plan', 'plan looks good', '评审通过', '批准计划', '锁定计划', '方案没问题，可以做了'. Checks the plan has a complete design, confirms approval with the person, then marks it Locked. Only after Locked may gsw-implement start; before that no code or artifact is touched. Does not code."
---

# gsw-approve

生命周期：`Idea` → `Draft` → `Locked`（`Done` 由 gsw-ship 标记）。

锁定是"方案先行"的闸门：方案没 `Locked` 之前不改任何代码与产物，只做方案（方案写在
plans 里）；实现必须等 gsw-implement 在 `Locked` 计划上进行。

## 步骤

1. 读计划（`doc/plans/records/<file>`，本地落后先 pull）。状态必须是 `Draft`；
   `Idea`（没成稿）就停下："先用 gsw-plan 把 Decision、验收检查和步骤写好"；
   已是 `Locked` 就告知无需重复审批。
2. 核对方案完整性，缺一样就停下并用 gsw-plan 补：
   - `## Decision` 有选定方案与理由（含关键参数：契约/接口/阈值）；
   - `## Acceptance checks` 是可运行的命令或可判定的判据，含一个预期 `FAIL` 用例；
   - `## Steps` 存在且覆盖交付；front matter `class`、`packages` 已填；
   - 有附件的，附件在同名 `_plan` 附件夹里且被正文引用。
3. 向这个人复述：目标一句话、关键 Decision、影响面（`packages`）、验收检查清单，
   然后问是否批准。只问这一次，得到明确批准才继续。
4. 批准后：front matter `status` 置 `Locked`（不改其它内容；要改方案就回 gsw-plan，
   改完状态回到 `Draft` 重新审批）。更新 `doc/plans/INDEX.md`：状态 `Locked`，
   下一步 = gsw-implement。单独提交（提交只含 `doc/plans/`，信息形如
   `plan(<slug>): approve`）并推送。
5. 回复：计划路径、锁定提交号、下一步（用 gsw-implement 实现）。

## 不要

- 不写代码、不改产品文件；方案有变回 gsw-plan（`Locked` → `Draft`）。
- 这个人没明确批准就不置 `Locked`。
- 不跳过第 2 步完整性核对。
- 不在功能分支上改计划；提交只含 `doc/plans/`。
