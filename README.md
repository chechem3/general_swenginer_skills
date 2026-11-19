# general_swenginer_skills

通用软件工程流程技能（个人版）：从 `vpel_motion/.agents/skills/` 的 `vas-*` 流程技能
提炼简化而成——去掉多评审人批准与结项流程，适合一个人管理自己的想法和实现，
可在任意仓库直接使用。

## 技能与生命周期

状态流：`Idea → Draft → Done`。想法记录即 `Idea`；写好设计即 `Draft`；gsw-ship 开完
MR/PR 标记 `Done`。不做的想法留在 `Idea` 即可，没有单独的关闭动作。

| 技能 | 作用 | 状态转换 |
|---|---|---|
| gsw-plan | 记录想法，并（同一或后续会话）写成可实施的小设计 | — → Idea → Draft |
| gsw-implement | 按 Draft 计划测试先行写代码；class S 小项可无计划 | 状态不变 |
| gsw-ship | 跑测试、更新文档、开 MR/PR、标记 Done | Draft → Done |
| gsw-status | 只读查询状态 | 不改任何文件 |

## 安装

把整个目录拷到目标仓库的 `.agents/skills/`（用户级则 `~/.agents/skills/`）。
也可以只拷需要的技能：每个 SKILL.md 自含它用到的约定，单独可用。

## 计划文件（plan file）

- 位置：`plans/records/YYYYMMDD_<slug>.md`（`slug`：小写字母、数字、`_`）。
- 索引：`plans/INDEX.md` 列出计划（slug、标题、状态、下一步），每次状态变更时更新。
- 计划变更单独提交（提交只含 `plans/`）；功能分支不得增改 `plans/`。

front matter 字段与章节（模板见 gsw-plan）：

| 字段 | 说明 |
|---|---|
| title / summary | 标题；一行摘要 |
| language | 计划正文语言（`en`/`zh`，缺省按提出者的消息语言）|
| status | Idea / Draft / Done |
| class | S（单模块小改，无需计划）/ M（单模块新功能）/ L（跨模块、契约、阈值、外部行为变更）|
| packages | 受影响模块/目录 |
| requirements | 需求 ID 列表 |
| tracker | 外部跟踪项 ID（Jira/Feishu/Issue…；暂无则 `pending`）|
| mr | gsw-ship 开完 MR/PR 后填链接 |

章节顺序：`## Why`（问题与来源，保留提出者原文）→ `## Ideas and options`（记录时填）
→ `## Decision`、`## Acceptance checks`（表：ID、检查、期望结果，含一个预期 FAIL 用例）、
`## Steps`（成稿时填）→ `## Result`（gsw-ship 时填：MR/PR、日期、验收结果）。

## 采用时的适配点

- 跟踪系统：`tracker` 字段按你们的 Jira/Feishu/Issue ID 约定使用。
- 分支命名：`feature|bugfix|refactor/<tracker>-<slug>`，从默认分支切出。
- 提交 trailers：`Plan: <slug>` 与 `Item: <tracker>`，用于追溯与发布说明。
- MR 工具：`glab`（GitLab）/`gh`（GitHub），任选。
- 存放方式：默认计划放仓库内 `plans/` 目录、随默认分支版本化；若要和代码完全隔离，
  可改为独立分支 + worktree 的挂载方案（VPEL 的做法），技能步骤不变，只换读写路径。

## 与 vas-* 的差异

| 去掉/替换 | 原因 |
|---|---|
| `ci/scripts/plan_tool.py` | 改为直接用 git + 文件编辑；检查由技能步骤本身承担 |
| `plans` 孤儿分支 + `doc/plans/` worktree | 简化为仓库内 `plans/` 目录 |
| **vas-approve（多评审人批准、approved_by、版本锁定）** | 移除：个人使用，自己对自己负责 |
| **vas-close（Implemented/Parked/Dropped/Superseded）** | 移除：完成由 gsw-ship 标记 Done；不做的想法留在 Idea |
| **vas-idea + vas-plan 分立** | 合并为一个 gsw-plan：记录与成稿是同一文件的先后两步 |
| Feishu（noam CLI）与固定 `feishu` 字段 | 泛化为 `tracker` 字段 |
| GitLab `glab` 专属流程 | 泛化为 MR/PR，任一平台工具 |
| class L 的 PRD §6.3 owner、ASD-STE100、固定 owner 人名、状态 `Locked` | 项目专属，删除 |
