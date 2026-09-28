# Issue tracker: GitHub

本仓库的 issue 和规格存放在 GitHub Issues 中。所有操作使用 `gh` CLI。

## 约定

- **创建 issue**：`gh issue create --title "..." --body "..."`。多行正文用 heredoc。
- **AI 贡献标记**：AI 提交的 issue 与 PR 标题以 `[AI Generated][<类型>]` 开头（Issue 用 `[FEAT]` / `[BUG]` / `[IDEA]` / `[RESEARCH]` / `[TASK]`）。AI 写的评论首行用 `> **[AI Generated]** 本评论由 AI 完成。` 或 `> **[AI Assisted]** 本评论由 AI 辅助完成。` 标记里的执行者身份按当前工作环境如实写。
- **读取 issue**：`gh issue view <number> --comments`。
- **列出 issue**：`gh issue list --state open --json number,title,labels`。
- **评论 / 标签 / 关闭**：`gh issue comment`、`gh issue edit --add-label/--remove-label`、`gh issue close <number> --comment "..."`。

## Pull request 作为分诊渠道

**PR 作为请求渠道：否。**

## GitHub Project

- Issue 和 PR 都加入本仓库的 GitHub Project "cspice-rs Issue Management"；Project 是工作状态的来源，label 只表达分类。
- 使用 `gh project item-list 5 --owner cislunarspace --format json --limit 1000` 查询项目项，使用 `gh project item-edit --id <item-id> --project-id <project-id> --field-id <field-id> --single-select-option-id <option-id>` 更新字段。
- 默认状态流转：`Inbox` → `Backlog` → `Ready` → `In progress` → `In review` → `Done` / `No action`。
- `Done` 对应 Issue 以 `Completed` 关闭；`No action` 对应 Issue 以 `Not planned` 关闭；重开的 Issue 回到 `Inbox`。
- `Priority` 使用 `P0`–`P3`，`Start Date` 由维护者维护。

### 工作流状态迁移

- 新 Issue：加入 Project，设为 `Inbox`。
- 分诊确认但未排期：`Backlog`；可开始：`Ready`。
- 开始实现：`In progress`；创建 PR：`In review`。
- PR 合并并验证完成：Issue 关闭原因为 `Completed`，Project 设为 `Done`。

### Project 配置

本仓库 Issue 统一进入 Project “cspice-rs Issue Management”（https://github.com/orgs/cislunarspace/projects/5），owner `cislunarspace`、number `5`。组织下另有 “cislunarspace Issue Management”（CODE-core 用）与 “qiao Issue Management”，与本仓库无关，不要混用。

ID 于 2026-10-24 建仓时经 `gh project field-list 5 --owner cislunarspace --format json` 实测记录。

- Project ID：`PVT_kwDOE3ZAg84Bk7Ni`
- Status 字段 ID：`PVTSSF_lADOE3ZAg84Bk7Nizhjp9d4`

  | 选项 | option ID |
  | --- | --- |
  | Inbox | `7200cfda` |
  | Backlog | `5551486e` |
  | Ready | `7e97a434` |
  | In progress | `4afd2b02` |
  | In review | `7aa7e4b5` |
  | Done | `49ab2e96` |
  | No action | `a5504eb5` |

- Priority 字段 ID：`PVTSSF_lADOE3ZAg84Bk7Nizhjp-Os`

  | 选项 | option ID |
  | --- | --- |
  | P0 | `fc26a5e6` |
  | P1 | `46e7c969` |
  | P2 | `c901161c` |
  | P3 | `3150b11b` |

- Start Date 字段 ID：`PVTF_lADOE3ZAg84Bk7Nizhjp-Ow`
