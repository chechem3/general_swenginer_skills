# general_swenginer_skills

个人版软件工程流程技能：在任意仓库里管理"想法 → 设计 → 锁定 → 实现 → 提交"的完整
生命周期。不依赖特定平台工具，拷进 `.agents/skills/` 即可使用。

## 技能与生命周期

状态流：`Idea → Draft → Locked → Done`。想法记录即 `Idea`；写好设计即 `Draft`；评审
批准后 gsw-approve 锁定即 `Locked`；gsw-ship 开完 MR/PR 标记 `Done`。

**研究轨**：`type: research` 的计划走 `Idea → Done`，跳过成稿、锁定与实现，由
gsw-research 执行：只读代码与产品，产出写 `doc/research/`。研究是方案先行的上游，
产出喂给 gsw-plan 的 Why/Options/Decision；要落地成功能就回 gsw-plan 开实现计划。

**方案先行（硬闸门）**：`Locked` 之前不改任何代码与产品文件，只做方案与研究——
方案写在 plans 里，研究产出写在 `doc/research/`；实现只有一条路径——gsw-implement
在 `Locked` 计划上做。方案要改就回 gsw-plan（`Locked` 退回 `Draft`），改完重新审批。

| 技能 | 作用 | 状态转换 |
|---|---|---|
| gsw-plan | 记录想法，并（同一或后续会话）写成可实施的小设计 | — → Idea → Draft |
| gsw-research | 只读梳理代码/链路/工具/外部知识，产出 doc/research/ 文档 | Idea → Done（type: research）|
| gsw-approve | 评审方案完整性，人批准后锁定为 Locked | Draft → Locked |
| gsw-implement | 按 Locked 计划测试先行写代码；class S 小项可无计划 | 状态不变 |
| gsw-ship | 跑测试、更新文档、开 MR/PR、标记 Done | Locked → Done |
| gsw-status | 只读查询状态 | 不改任何文件 |

## 安装

把整个目录拷到目标仓库的 `.agents/skills/`（用户级则 `~/.agents/skills/`）。
也可以只拷需要的技能：每个 SKILL.md 自含它用到的约定，单独可用。

## 计划文件（plan file）

- 位置：`doc/plans/records/YYYYMMDD_<slug>.md`（`slug`：小写字母、数字、`_`）。
- 附件：计划的扩充材料与附件放同名附件夹 `doc/plans/records/<计划文件名去 .md>_plan/`，
  正文用相对路径引用，随计划一起提交。
- 索引：`doc/plans/INDEX.md` 列出计划（slug、标题、状态、下一步），每次状态变更时更新。
- 计划变更单独提交（提交只含 `doc/plans/`；研究完成例外：`doc/plans/` +
  `doc/research/`）；功能分支不得增改 `doc/plans/`。

front matter 字段与章节（模板见 gsw-plan）：

| 字段 | 说明 |
|---|---|
| title / summary | 标题；一行摘要 |
| language | 计划正文语言（`en`/`zh`，跟随提出者的消息语言，不做英文兜底）|
| status | Idea / Draft / Locked / Done |
| class | S（单模块小改，无需计划）/ M（单模块新功能）/ L（跨模块、契约、阈值、外部行为变更）|
| type | `research`（研究类，走 gsw-research）/ 留空（实现类）|
| packages | 受影响模块/目录 |
| requirements | 需求 ID 列表 |
| tracker | 外部跟踪项 ID（Jira/Feishu/Issue…；暂无则 `pending`）|
| mr | gsw-ship 开完 MR/PR 后填链接 |

章节顺序：`## Why`（问题与来源，保留提出者原文）→ `## Ideas and options`（记录时填）
→ `## Decision`、`## Acceptance checks`（表：ID、检查、期望结果，含一个预期 FAIL 用例）、
`## Steps`（成稿时填）→ `## Result`（gsw-ship 时填：MR/PR、日期、验收结果）。
研究计划（`type: research`）只填 `## Why` 与 `## Result`（gsw-research 收尾时填
产出路径），中间章节留空。

## 采用时的适配点

- 跟踪系统：`tracker` 字段按你们的 Jira/Feishu/Issue ID 约定使用。
- 分支命名：`feature|bugfix|refactor/<tracker>-<slug>`，从默认分支切出。
- 提交 trailers：`Plan: <slug>` 与 `Item: <tracker>`，用于追溯与发布说明。
- MR 工具：`glab`（GitLab）/`gh`（GitHub），任选。
- 存放方式：默认计划放仓库内 `doc/plans/` 目录、随默认分支版本化；若要和代码完全隔离，
  可改为独立分支 + worktree 挂载的方案，技能步骤不变，只换读写路径。
