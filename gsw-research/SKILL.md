---
name: gsw-research
description: "USE WHEN a person wants an investigation, comparison, or 梳理 of code, a signal/link chain, architecture, a tool, or external knowledge, producing a written findings document instead of code. Triggers: 'research this', 'investigate', 'how does X work', 'compare options', '梳理', '调研', '分析一下', '看看X是怎么走的', '整理一下链路'. Read-only on code and products; writes only doc/research/ (findings doc + attachments) and keeps a plan record (type: research, Idea → Done). Does not decide designs — that is gsw-plan."
---

# gsw-research

研究轨：`type: research` 的计划走 `Idea` → `Done`，跳过成稿、锁定与实现。
研究只读代码与产品、只写 `doc/research/`，是"方案先行"的上游：产出喂给
gsw-plan 的 Why/Options/Decision，也可作为计划附件被引用。要落地成功能，
回 gsw-plan 开实现计划。

## 步骤

1. 入口两种：这个人给主题直接研究，或给已有 `type: research` 计划的 slug。
   主题明显是梳理/调研/对比（产出是认识，不是代码）就按研究处理；拿不准
   （既像调研又像要做功能）就用一个问题确认。任何语言都可以；给了聊天/文档
   链接且本机有对应 CLI 就先读内容。
2. 留记录（必须）：没有计划记录就先建——提议 slug（小写字母、数字、`_`）与
   标题，创建 `doc/plans/records/YYYYMMDD_<slug>.md`，模板同 gsw-plan，front
   matter 置 `type: research`、`status: Idea`，只填 `## Why`（这个人的原话与
   触发来源，附链接），其余章节留空。更新 `doc/plans/INDEX.md`（下一步 =
   用 gsw-research 执行），单独提交并推送（就在当前分支提交，不开分支；
   提交只含 `doc/plans/`，信息形如 `plan(<slug>): idea`）。
3. 只读调查：读代码、配置、文档，外部知识用网络检索，边查边记证据。
   可以跑只读命令验证（读文件、列目录、跑分析看输出）；需要临时脚本做数据
   分析就放系统临时目录，不进仓库，要保留的数据/工具第 4 步作为附件放进
   研究附件目录。这一步前后 `git status` 必须一致：产品代码与 `doc/research/`、
   `doc/plans/` 之外的文件一个都不改。
4. 写产出：`doc/research/YYYYMMDDHHMM_<标题>.md`（时间戳用当前时刻）。结构
   建议：结论先行（一段话说清查到了什么）→ 证据与细节（引用带 `file:line`
   或来源链接）→ 开放问题（没查清的、需要别人确认的）。全文用这个人当前
   消息的语言。附件、数据、代码镜像放同名目录
   `doc/research/YYYYMMDDHHMM_<标题>/`，正文用相对路径引用；镜像只增不改，
   不覆盖已有文件。
5. 收尾：计划 `status` 置 `Done`，`## Result` 写一两行（产出路径、日期）；
   更新 `doc/plans/INDEX.md`（状态 Done，下一步 = 要落地就用 gsw-plan）。
   单独提交并推送（提交只含 `doc/plans/` + `doc/research/`，信息形如
   `research(<slug>): done`，trailers `Plan: <slug>`；推的是当前分支，
   完成即推，仓库没配远端就说明一声，提交先留在本地）。
6. 回复：产出路径、关键结论两三句、开放问题；如果结论指向一个可做的功能，
   提示"要用 gsw-plan 开实现计划"。

## 不要

- 不开分支：研究全程就在当前分支上进行，不建 `research/<slug>` 之类的分支，
  也不切分支；提交与推送都落在当前分支。
- 不改代码、不改产品文件；`doc/research/` 与 `doc/plans/` 之外一个文件都
  不动，代码镜像只增不改。
- 不做设计决策、不定契约/阈值/方案——研究只陈述事实、选项与证据；
  选方案是 gsw-plan 的事。
- 不走 gsw-approve/gsw-implement：研究不改产品，不需要锁定闸门；也不给
  研究计划写 Decision、验收检查或 Steps。
- 不留记录不收工：一句话的梳理也要走第 2 步建记录、第 5 步标 Done。
- 一个研究只讲一个主题；研究中冒出的新主题记进开放问题或另开记录，
  不顺手扩题。
