# qq-bridge 紧急备份（2026-11-12）

上游仓库 [Derpyu520/qq-bridge](https://github.com/Derpyu520/qq-bridge) 已宣布停止维护并计划于 2026-11-18 前后归档。
本仓库 [MurphyPotato/qq-bridge-backup](https://github.com/MurphyPotato/qq-bridge-backup) 是它的完整紧急备份，备份时间 **2026-11-12**。

## 一、已备份内容

| 内容 | 状态 |
| --- | --- |
| 全部分支（`main` / `dev` / `docs/full-review` / `feat/dsh-0.2.2-adaptation`） | ✅ 与上游 SHA 完全一致 |
| 全部标签（10 个，v0.1.0 → v0.2.2-rc1） | ✅ |
| 全部 Release（10 个：标题、说明正文、预发布标记、资产） | ✅ 逐字一致，`latest` 指向 v0.2.1-r2 |
| Release 资产 `project-intro.mp4`（11,463,561 字节） | ✅ SHA-256 一致 |
| Issues（84 个）与 Pull Requests（95 个）及其评论（141 条）、Review 评论（2 条） | ✅ 元数据 JSON，见 `_backup/` 目录 |
| PR head refs（112 个，含未合并分支） | ✅ 打包在 `qq-bridge-full-2026-11-12.bundle` 中 |
| Discussions / Wiki | 上游分别为 0 条 / 未启用 |

> GitHub 不允许把 `refs/pull/*` 直接推送到 fork，因此 PR 相关提交用完整 git bundle 保存（见下方「恢复」）。

## 二、目录说明（`_backup/`）

- `manifest.json` —— 备份清单（时间、计数、文件说明）
- `repo.json` —— 仓库元数据（描述、topics、star/fork 数等）
- `refs.txt` —— 备份时刻全部分支/标签的 SHA
- `releases.json` —— 10 个 Release 的完整元数据（含资产信息）
- `issues.json` —— 84 个 Issue
- `issues-and-prs.json` —— 上游 issue 列表接口的原始返回（含 PR 条目，179 条）
- `issue-comments.json` —— Issue/PR 会话评论（141 条）
- `pull-requests.json` —— 95 个 PR
- `pull-reviews.json` —— PR Review 汇总（2 条）
- `pull-review-comments.json` —— PR 行内 Review 评论（2 条）
- `discussions.json` —— Discussions GraphQL 快照（totalCount = 0）

## 三、恢复方法

### 1. 普通克隆（分支 + 标签 + Releases）

```bash
git clone https://github.com/MurphyPotato/qq-bridge-backup.git qq-bridge
cd qq-bridge
git branch -a      # 4 个分支
git tag            # 10 个标签
```

Releases 页面：<https://github.com/MurphyPotato/qq-bridge-backup/releases>

### 2. 从 bundle 恢复「全部 refs」（含 112 个 PR head refs）

下载 Release `backup-2026-11-12` 中的 `qq-bridge-full-2026-11-12.bundle`，然后：

```bash
git clone --mirror qq-bridge-full-2026-11-12.bundle qq-bridge.git
cd qq-bridge.git
git for-each-ref | wc -l   # 应为 126 个 refs（4 branches + 10 tags + 112 pull refs）
git log --oneline main -1  # 781e02f24e17004d3821c1b71501a442e07ed756
```

也可以先 clone 普通仓库，再合并 bundle：

```bash
git clone https://github.com/MurphyPotato/qq-bridge-backup.git qq-bridge
cd qq-bridge
git fetch /path/to/qq-bridge-full-2026-11-12.bundle 'refs/pull/*:refs/pull/*'
```

### 3. 校验

- bundle SHA-256：`06ebdd1ff0c42046a04fecc597105f5563df620cea7841323afc94e4a2a40881`
- 上游 `main`：`781e02f24e17004d3821c1b71501a442e07ed756`（备份时）
- 资产 `project-intro.mp4` SHA-256：`cfaadb71ad3ee297a5ea0518240d8055db974358bbf5ac7bc1edcd2b56d32992`

## 四、说明

- 这是纯粹的自救备份，版权与许可归属上游作者（详见仓库 `LICENSE`）。
- 上游若恢复/继续维护，直接 `git pull` 对应分支即可重新同步。
- Issues / PR 的 JSON 只保存数据，不包含 GitHub 上的可交互状态（标签颜色、时间线事件流等）。
