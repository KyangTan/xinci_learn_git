# NEW COMMIT
# 🚀 Git 学习指南 | Complete Git Learning Guide

> 从零开始掌握 Git 版本控制 | Master Git Version Control from Scratch

---

## 📚 目录 | Table of Contents | STAGED

1. [什么是 Git？](#什么是-git)
2. [安装配置](#安装配置)
3. [核心概念](#核心概念)
4. [基础命令](#基础命令)
5. [分支管理](#分支管理)
6. [远程仓库](#远程仓库)
7. [常用工作流](#常用工作流)
8. [进阶技巧](#进阶技巧)
9. [最佳实践](#最佳实践)

---

## 什么是 Git？

**Git** 是一个分布式版本控制系统，用于跟踪文件的更改历史。它可以帮助你：

- ✅ 追踪代码的每一次修改
- ✅ 多人协作开发
- ✅ 回滚到任意历史版本
- ✅ 创建分支进行实验性开发

---

## 安装配置

### 安装 Git

**macOS:**
```bash
# 使用 Homebrew
brew install git

# 或直接下载安装包
# https://git-scm.com/download/mac
```

**Windows:**
```bash
# 下载安装包
# https://git-scm.com/download/win

# 或使用 winget
winget install Git.Git
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install git
```

### 初始配置

安装完成后，配置你的用户信息：

```bash
# 设置用户名
git config --global user.name "Your Name"

# 设置邮箱
git config --global user.email "your.email@example.com"

# 设置默认编辑器 (可选)
git config --global core.editor "code --wait"  # VS Code
git config --global core.editor "vim"          # Vim

# 查看所有配置
git config --list
```

---

## 核心概念

### 三个区域

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   工作区         │     │   暂存区         │     │   仓库          │
│  Working Dir    │ ──▶ │  Staging Area   │ ──▶ │   Repository    │
│                 │ add │                 │commit│                 │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

| 区域 | 英文名 | 说明 |
|------|--------|------|
| 工作区 | Working Directory | 你实际编辑文件的地方 |
| 暂存区 | Staging Area / Index | 准备提交的文件快照 |
| 仓库 | Repository | 提交历史的永久存储 |

### 文件状态

```
Untracked ──▶ Staged ──▶ Committed
    │           ▲           │
    │    add    │           │ modify
    └───────────┴───────────┘
```

- **Untracked**: 新文件，Git 不追踪
- **Modified**: 已修改，但未暂存
- **Staged**: 已暂存，准备提交
- **Committed**: 已提交到仓库

---

## 基础命令

### 创建仓库

```bash
# 在当前目录初始化新仓库
git init

# 克隆远程仓库
git clone https://github.com/user/repo.git

# 克隆到指定目录
git clone https://github.com/user/repo.git my-folder
```

### 查看状态

```bash
# 查看仓库状态
git status

# 简洁模式
git status -s
```

### 添加文件到暂存区

```bash
# 添加单个文件
git add filename.txt

# 添加多个文件
git add file1.txt file2.txt

# 添加所有修改的文件
git add .

# 添加所有 .js 文件
git add *.js

# 交互式添加
git add -p
```

### 提交更改

```bash
# 提交并添加消息
git commit -m "feat: add new feature"

# 添加并提交 (跳过 git add)
git commit -am "fix: bug fix"

# 修改上一次提交
git commit --amend -m "new message"
```

### 查看历史

```bash
# 查看提交历史
git log

# 简洁的单行显示
git log --oneline

# 图形化显示分支
git log --oneline --graph --all

# 查看最近 5 次提交
git log -5

# 查看某个文件的历史
git log -- filename.txt
```

### 查看差异

```bash
# 查看工作区与暂存区的差异
git diff

# 查看暂存区与仓库的差异
git diff --staged

# 查看两个提交之间的差异
git diff commit1 commit2

# 查看某个文件的差异
git diff -- filename.txt
```

---

## 分支管理

### 分支基础

```bash
# 查看所有分支
git branch

# 查看所有分支 (包括远程)
git branch -a

# 创建新分支
git branch feature-login

# 切换分支
git checkout feature-login

# 创建并切换 (推荐)
git checkout -b feature-login
# 或使用新命令
git switch -c feature-login

# 删除分支
git branch -d feature-login

# 强制删除未合并的分支
git branch -D feature-login

# 重命名分支
git branch -m old-name new-name
```

### 合并分支

```bash
# 合并分支到当前分支
git merge feature-login

# 合并时不使用 fast-forward
git merge --no-ff feature-login

# 取消合并
git merge --abort
```

### 变基 (Rebase)

```bash
# 变基到 main 分支
git rebase main

# 交互式变基 (修改历史)
git rebase -i HEAD~3

# 取消变基
git rebase --abort

# 继续变基 (解决冲突后)
git rebase --continue
```

---

## 远程仓库

### 配置远程仓库

```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/user/repo.git

# 修改远程仓库 URL
git remote set-url origin https://github.com/user/new-repo.git

# 删除远程仓库
git remote remove origin

# 重命名远程仓库
git remote rename origin upstream
```

### 推送与拉取

```bash
# 推送到远程
git push origin main

# 首次推送并设置上游分支
git push -u origin main

# 强制推送 (谨慎使用!)
git push --force
git push -f

# 拉取远程更新
git pull origin main

# 获取远程更新但不合并
git fetch origin

# 拉取并变基
git pull --rebase origin main
```

---

## 常用工作流

### 1. 功能分支工作流 (Feature Branch)

```bash
# 1. 从 main 创建功能分支
git checkout main
git pull origin main
git checkout -b feature/user-auth

# 2. 开发功能，多次提交
git add .
git commit -m "feat: add login form"
git add .
git commit -m "feat: add validation"

# 3. 推送到远程
git push -u origin feature/user-auth

# 4. 创建 Pull Request (在 GitHub/GitLab 上)

# 5. 合并后删除分支
git checkout main
git pull origin main
git branch -d feature/user-auth
```

### 2. Git Flow 工作流

```
main ────●────────────────●────────────────●──▶
         │                │                │
develop ─●────●────●─────●────●────●──────●──▶
              │    │          │    │
feature      ●────●          ●────●
```

### 3. Commit 信息规范

```
<type>(<scope>): <subject>

<body>

<footer>
```

**类型 (type):**
| 类型 | 说明 |
|------|------|
| `feat` | 新功能 |
| `fix` | 修复 bug |
| `docs` | 文档更新 |
| `style` | 代码格式 (不影响功能) |
| `refactor` | 重构 |
| `test` | 测试相关 |
| `chore` | 构建/工具相关 |

**示例:**
```bash
git commit -m "feat(auth): add JWT authentication"
git commit -m "fix(api): resolve null pointer exception"
git commit -m "docs: update README with setup instructions"
```

---

## 进阶技巧

### 暂存工作 (Stash)

```bash
# 暂存当前更改
git stash

# 暂存并添加描述
git stash save "WIP: working on feature"

# 查看所有暂存
git stash list

# 恢复最近的暂存
git stash pop

# 恢复指定暂存
git stash apply stash@{2}

# 删除暂存
git stash drop stash@{0}

# 清除所有暂存
git stash clear
```

### 撤销操作

```bash
# 撤销工作区的修改
git checkout -- filename.txt
git restore filename.txt  # 新命令

# 撤销暂存
git reset HEAD filename.txt
git restore --staged filename.txt  # 新命令

# 撤销提交 (保留更改)
git reset --soft HEAD~1

# 撤销提交 (放到工作区)
git reset --mixed HEAD~1  # 默认

# 撤销提交 (丢弃更改)
git reset --hard HEAD~1

# 创建一个新的提交来撤销之前的提交
git revert commit-hash
```

### 标签 (Tag)

```bash
# 创建轻量标签
git tag v1.0.0

# 创建带注释的标签
git tag -a v1.0.0 -m "Release version 1.0.0"

# 查看所有标签
git tag

# 推送标签到远程
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
```

### Cherry-pick

**Cherry-pick** 允许你将其他分支的特定提交应用到当前分支，而不需要合并整个分支。这对于选择性应用提交非常有用。

**基本用法:**
```bash
# 将指定提交应用到当前分支
git cherry-pick commit-hash

# 不自动提交 (只应用更改到工作区)
git cherry-pick --no-commit commit-hash

# 应用多个提交 (按顺序应用)
git cherry-pick commit-hash1 commit-hash2 commit-hash3

# 应用一个范围的提交 (不包含 start-commit)
git cherry-pick start-commit..end-commit

# 应用一个范围的提交 (包含 start-commit)
git cherry-pick start-commit^..end-commit
```

**常用选项:**
```bash
# 只应用更改但不提交 (需要手动提交)
git cherry-pick --no-commit commit-hash

# 编辑提交信息
git cherry-pick -e commit-hash
git cherry-pick --edit commit-hash

# 不创建提交，只更新工作区和暂存区
git cherry-pick -n commit-hash
git cherry-pick --no-commit commit-hash

# 如果提交已经存在，跳过
git cherry-pick --ff commit-hash

# 即使提交已经存在，也创建新的提交
git cherry-pick --no-ff commit-hash
```

**处理冲突:**
```bash
# 如果发生冲突，解决后继续
git cherry-pick --continue

# 跳过当前提交
git cherry-pick --skip

# 取消 cherry-pick 操作
git cherry-pick --abort
```

**实际应用场景:**

1. **从其他分支应用 bug 修复:**
   ```bash
   # 在 main 分支上应用 feature 分支的某个 bug 修复
   git checkout main
   git cherry-pick abc123  # feature 分支上的修复提交
   ```

2. **选择性应用提交:**
   ```bash
   # 只应用某些提交，而不是合并整个分支
   git checkout release
   git cherry-pick commit1 commit2 commit3
   ```

3. **从已删除的分支恢复提交:**
   ```bash
   # 找到提交哈希
   git log --all --oneline
   # 应用该提交
   git cherry-pick commit-hash
   ```

**注意事项:**
- ⚠️ Cherry-pick 会创建新的提交，即使内容相同，提交哈希也会不同
- ⚠️ 如果提交依赖其他提交，可能需要手动处理依赖关系
- ⚠️ 频繁使用 cherry-pick 可能导致提交历史混乱，优先考虑 merge 或 rebase
- ✅ 适合紧急修复、热修复等场景
- ✅ 适合从已删除的分支恢复特定提交

### 查找问题 (Bisect)

```bash
# 开始二分查找
git bisect start

# 标记当前版本有问题
git bisect bad

# 标记某个版本正常
git bisect good commit-hash

# Git 会自动切换版本，你需要测试并标记
git bisect good  # 或 git bisect bad

# 找到问题后结束
git bisect reset
```

---

## 最佳实践

### ✅ 推荐做法

1. **频繁提交**: 小步提交，每次提交只做一件事
2. **有意义的提交信息**: 清晰描述改动内容
3. **使用分支**: 新功能/修复都在分支上开发
4. **拉取前先提交**: 避免冲突时丢失代码
5. **定期推送**: 备份代码，方便协作
6. **代码审查**: 使用 Pull Request 进行代码审查

### ❌ 避免做法

1. 不要提交敏感信息 (密码、密钥)
2. 不要在 main 分支直接开发
3. 不要强制推送到共享分支
4. 不要提交未经测试的代码

### 常用 .gitignore 模板

```gitignore
# 依赖目录
node_modules/
vendor/
venv/

# 构建产物
dist/
build/
*.o
*.exe

# IDE 配置
.idea/
.vscode/
*.swp

# 系统文件
.DS_Store
Thumbs.db

# 环境变量
.env
.env.local

# 日志
*.log
logs/

# 临时文件
*.tmp
*.temp
```

---

## 🎯 快速参考卡片

### 日常使用

```bash
git status              # 查看状态
git add .               # 暂存所有
git commit -m "msg"     # 提交
git push                # 推送
git pull                # 拉取
```

### 分支操作

```bash
git branch              # 查看分支
git checkout -b name    # 创建并切换
git merge name          # 合并分支
git branch -d name      # 删除分支
```

### 撤销操作

```bash
git restore file        # 撤销工作区修改
git restore --staged f  # 撤销暂存
git reset --soft HEAD~1 # 撤销提交(保留)
git reset --hard HEAD~1 # 撤销提交(丢弃)
```

---

## 📖 学习资源

- [Pro Git 官方书籍 (中文)](https://git-scm.com/book/zh/v2)
- [Git 官方文档](https://git-scm.com/doc)
- [Learn Git Branching 交互式教程](https://learngitbranching.js.org/?locale=zh_CN)
- [GitHub Skills](https://skills.github.com/)

---

> 💡 **提示**: 学习 Git 最好的方式是实践！创建一个测试仓库，尝试所有命令，不要害怕犯错。

---

*Created with ❤️ for Git learners*


