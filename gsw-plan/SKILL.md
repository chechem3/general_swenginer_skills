---
name: gsw-plan
description: "USE WHEN a person wants to record a new idea, feature thought, or problem, or turn it into a small design ready to implement. Triggers: 'capture this idea', 'new idea', 'write the plan', 'design this', 'note this idea', '记录想法', '新想法', '写计划', '做设计', '记一下这个想法'. Creates doc/plans/records/YYYYMMDD_<slug>.md, fills Why and options first (status Idea), then Decision, Acceptance checks, and Steps (status Draft). Does not code."
---

# gsw-plan

生命周期：— → `Idea` → `Draft` → `Locked`（`Locked` 由 gsw-approve 标记，`Done` 由 gsw-ship 标记）。

一个计划文件三个阶段：先**记录**（`Idea`），再**成稿**（`Draft`），批准后由 gsw-approve
**锁定**（`Locked`）。方案先行：`Locked` 之前不改任何代码与产物，只做方案、方案写在
plans 里；实现必须等 gsw-implement 在 `Locked` 计划上做。这个人只要记录就到
第 5 步停下；想继续设计就接着做完。两次可以在同一个会话里，也可以隔天补成稿。

## 步骤

1. 从这个人的话里取想法；如果给了聊天/文档链接且本机有对应 CLI，读取其内容再用。
   任何语言都可以。
2. 提议一个简短 slug（小写字母、数字、`_`）、标题和计划语言。计划语言跟随这个人当前
   消息的语言：中文表达就整份文档用中文（`title`、`summary`、Why、Decision、验收检查、
   Steps 全部中文，不做英文兜底），英文表达就全用英文；仅当这个人明确点名语言时才用
   点名的语言。章节锚点（`## Why` 等）和 slug 格式保持固定，不随语言变。
   只在它们不明确时用一个问题确认。
3. 创建 `doc/plans/records/YYYYMMDD_<slug>.md`（目录不存在就先建），内容为模板：

   ````markdown
   ---
   title: <标题>
   summary: <一行摘要>
   status: Idea          # Idea / Draft / Locked / Done
   class:                # S / M / L（成稿时填）
   type:                 # research（研究类，走 gsw-research）或留空（实现类）
   language: <en|zh>
   packages: []
   requirements: []
   tracker: pending
   mr:                   # gsw-ship 开完 MR/PR 后填链接
   ---

   ## Why

   ## Ideas and options

   ## Decision

   ## Acceptance checks

   ## Steps

   ## Result
   ````

4. 只填：`## Why`（问题与触发来源，用这个人的原话，中文原样保留，附链接）和
   `## Ideas and options`（这个人点名的选项，一行一个）。不要发明设计。
   主题明显是研究类（梳理/调研/对比，产出是认识不是代码）就在 front matter
   置 `type: research`。
5. **记录到此为止时**：更新 `doc/plans/INDEX.md`（slug、标题、状态、下一步），提交并推送
   （提交只含 `doc/plans/`，信息形如 `plan(<slug>): idea`），回复文件路径和
   下一步（实现类"继续用 gsw-plan 成稿"；研究类"用 gsw-research 执行"）。然后停。
6. **成稿**：`type: research` 的计划到此为止，回复"用 gsw-research 执行"。
   实现类先定级——
   - **S**：单模块内的缺陷修复或重构，不改契约、阈值、外部行为。告知：无需计划，
     直接用 gsw-implement 做一个小工作项（tracker + 一句话目标）。到此为止。
   - **M**：单模块新功能。**L**：跨模块、契约/接口、阈值、外部行为变更。
7. 只问缺的东西，一次一问，并附你的建议答案：选定的选项及理由；后果；受影响的
   `packages`；验收检查（写成可运行的命令或可判定的判据，含一个预期 `FAIL` 用例）。
8. 写 `## Decision`、`## Acceptance checks`（表：ID、检查、期望结果）和简短的
   `## Steps`，用计划的 `language` 写全部内容（含 `title`/`summary`，不中英混排）。
   保留 `## Why` 里这个人的原文。front matter 填
   `class`、`packages`、`requirements`、`tracker`，`status` 置 `Draft`。
9. 更新 `doc/plans/INDEX.md`，提交并推送（提交只含 `doc/plans/`，信息形如
   `plan(<slug>): draft`）。
10. 回复：class、文件路径，下一步（用 gsw-approve 评审锁定）。

## 计划附件

- 计划的扩充材料与附件（接口表、DBC 产物、评审记录、数据等）放同名附件夹
  `doc/plans/records/<计划文件名去 .md>_plan/`，正文用相对路径引用。
- 附件夹随计划走同样的提交规矩：单独提交，只含 `doc/plans/`。

## 不要

- 不写代码；这个人只说"记一下"时不强行成稿。
- 不给研究类（`type: research`）计划写 Decision、验收检查或 Steps；研究走
  gsw-research。
- 不发明这个人没认可的设计或跑不了的验收检查。
- 一个计划只讲一个主题；新主题开新计划。
- 计划文件不在功能分支上写；提交只含 `doc/plans/`。
