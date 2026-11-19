# general_swenginer_skills

个人版软件工程流程技能：在任意仓库里管理"想法 → 设计 → 实现 → 提交"的完整生命周期。
不依赖特定平台工具，拷进 `.agents/skills/` 即可使用。

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

- 位置：`doc/plans/records/YYYYMMDD_<slug>.md`（`slug`：小写字母、数字、`_`）。
- 索引：`doc/plans/INDEX.md` 列出计划（slug、标题、状态、下一步），每次状态变更时更新。
- 计划变更单独提交（提交只含 `doc/plans/`）；功能分支不得增改 `doc/plans/`。

front matter 字段与章节（模板见 gsw-plan）：

| 字段 | 说明 |
|---|---|
| title / summary | 标题；一行摘要 |
| language | 计划正文语言（`en`/`zh`，跟随提出者的消息语言，不做英文兜底）|
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
- 存放方式：默认计划放仓库内 `doc/plans/` 目录、随默认分支版本化；若要和代码完全隔离，
  可改为独立分支 + worktree 挂载的方案，技能步骤不变，只换读写路径。
