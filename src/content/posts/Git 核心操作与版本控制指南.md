---
title: "Git 核心操作与版本控制指南"
published: 2026-10-22
description: "Git 基础、分支管理与 TortoiseGit 使用指南"
---

# Git 与开发工具使用指南

## 一、 Gitee 与 TortoiseGit

### Gitee
国内的代码托管平台

### TortoiseGit
专为 Windows 系统设计的 **Git 版本控制系统的图形化客户端（Shell 界面）**

---

## 二、 Git 基础教程与核心操作

### 1. 初始配置
首次使用或提交前需要配置用户名和邮箱：
```bash
git config --global user.name '你的名字'
git config --global user.email '你的邮箱'
```

### 2. 基础操作流程
* **初始化仓库**
  ```bash
  git init
  ```
* **查看仓库状态**（工作区、暂存区、Commit 区）
  ```bash
  git status
  ```
* **添加到暂存区**
  ```bash
  git add .
  ```
* **提交到版本库 (Commit)**
  ```bash
  git commit -m '提交注释说明'
  ```
* **查看 Commit 历史**
  ```bash
  git log
  ```
  *(包含 Commit ID、作者用户名和邮箱、提交日期)*
* **查看具体修改内容 (Diff)**
  ```bash
  git diff
  ```
  *(快捷键：`j` 下一行，`k` 上一行，`f` 下一页，`b` 上一页)*

### 3. 撤销与回滚操作

1. **工作区修改撤销**（放弃修改）：
   ```bash
   git restore <file>       # 撤销单个文件
   git restore .            # 撤销当前目录所有修改
   ```
2. **移出暂存区**（将已 `add` 的文件移出暂存盘）：
   ```bash
   git restore --staged <file>
   ```
3. **版本回滚 (`reset`)**：
   * `HEAD` 表示当前最新的 Commit 位置。
   * `--hard` 表示将工作区、暂存区全部回滚：
   ```bash
   git reset --hard HEAD~1
   ```
4. **反向提交 (`revert`)**：
   * 新建一个与指定 Commit 相反的提交来抵消修改：
   ```bash
   git revert HEAD
   ```
   *(进入编辑器后，保持默认即可，输入 `:wq` 保存并退出)*

### 4. 忽略文件配置
* 创建 `.gitignore` 文件。
* 凡在此文件内声明的文件或文件夹，Git 都会自动忽略。
* **注意**：`.gitignore` 文件本身也是项目的一部分，需要提交到版本库中。

---

## 三、 远程仓库与免密登录

### 1. 配置免密登录 (SSH)
* 配置 GitHub SSH Key 实现免密安全连接。

### 2. 远程克隆与三步走
* **克隆远程仓库**：
  ```bash
  git clone 项目地址 自定义文件夹名称
  ```
* **经典日常三步走**：
  ```bash
  git add .
  git commit -m "注释"
  git push
  ```

---

## 四、 分支（Branch）管理与进阶技巧

分支可以看作贴在 Commit 上面的标签（默认通常为 `main` 或 `master`）。新分支独立于主分支，后续可进行合并。

### 1. 分支常用命令
* **查看所有分支**：
  ```bash
  git branch
  ```
  *(带 `*` 号的表示当前所在分支)*
* **创建新分支**：
  ```bash
  git branch <branch_name>
  ```
* **切换分支**：
  ```bash
  git switch <branch_name>
  ```
* **创建并直接切换分支**：
  ```bash
  git switch -c <branch_name>
  ```
* **查看精简 Commit 历史**：
  ```bash
  git log --oneline
  ```
* **合并分支**（将指定分支合并到**当前**分支）：
  ```bash
  git merge <branch_name>
  ```
* **删除分支**：
  ```bash
  git branch -d <branch_name>
  ```

### 2. 进阶操作

* **处理合并冲突**：
  手动解决冲突文件中的冲突内容后，重新执行 `git add` 并提交一个新的 `commit` 完成合并。
* **变基 (`rebase`)**：
  让分支随时跟进主线，改变分支的基底。
  ```bash
  git rebase master
  ```
  过程中若遇冲突，解决后执行：
  ```bash
  git rebase --continue
  ```
* **暂存工作区 (`stash`)**：
  将当前未完成的改动临时存入暂存区。
  ```bash
  git stash        # 暂存当前改动
  git stash pop    # 重新取回并应用暂存的内容
  ```
* **多工作区管理 (`worktree`)**：
  新开一个工作区（有独立的暂存区，但共享 Commit 历史）。
  ```bash
  git worktree add -b <分支名字> ../<目录名字> master
  ```
  *基于 `master` 分支在父级目录创建新工作区。*
  *注：若对应分支正在其他工作区打开，会有特殊标记。*
  * **删除工作区**：
    ```bash
    git worktree remove <需要删除的目录>
    ```
