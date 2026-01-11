# 🐙 GitHub 学习指南 | Complete GitHub Learning Guide

> 从零开始掌握 GitHub 协作开发 | Master GitHub Collaboration from Scratch

---

## 📚 目录 | Table of Contents

1. [什么是 GitHub？](#什么是-github)
2. [账户设置](#账户设置)
3. [仓库管理](#仓库管理)
4. [协作功能](#协作功能)
5. [Pull Request 工作流](#pull-request-工作流)
6. [Issues 管理](#issues-管理)
7. [GitHub Actions](#github-actions)
8. [GitHub Pages](#github-pages)
9. [GitHub CLI](#github-cli)
10. [最佳实践](#最佳实践)

---

## 什么是 GitHub？

**GitHub** 是一个基于 Git 的代码托管和协作平台。它可以帮助你：

- ✅ 托管代码仓库
- ✅ 多人协作开发
- ✅ 代码审查和讨论
- ✅ 项目管理 (Issues, Projects)
- ✅ 自动化工作流 (GitHub Actions)
- ✅ 免费托管静态网站 (GitHub Pages)
- ✅ 发现和贡献开源项目

### Git vs GitHub

```
┌─────────────────┐         ┌─────────────────┐
│      Git        │         │     GitHub       │
│  版本控制工具    │   ──▶   │   代码托管平台    │
│  本地运行        │         │   云端服务        │
│  命令行工具      │         │   Web 界面       │
└─────────────────┘         └─────────────────┘
```

| 特性 | Git | GitHub |
|------|-----|--------|
| 类型 | 工具 | 平台 |
| 位置 | 本地 | 云端 |
| 协作 | 需要服务器 | 内置协作功能 |
| 界面 | 命令行 | Web + CLI |

---

## 账户设置

### 创建 GitHub 账户

1. 访问 [github.com](https://github.com)
2. 点击 "Sign up"
3. 输入用户名、邮箱和密码
4. 验证邮箱地址
5. 完成账户设置

### 配置 SSH 密钥 (推荐)

SSH 密钥可以让你无需每次输入密码就能推送代码。

**生成 SSH 密钥:**

```bash
# 生成新的 SSH 密钥
ssh-keygen -t ed25519 -C "your.email@example.com"

# 如果系统不支持 ed25519，使用 RSA
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

# 按 Enter 使用默认路径
# 设置密码 (可选，但推荐)
```

**添加到 SSH agent:**

```bash
# 启动 ssh-agent
eval "$(ssh-agent -s)"

# 添加密钥到 agent (macOS)
ssh-add ~/.ssh/id_ed25519
# 或 (Linux)
ssh-add ~/.ssh/id_rsa
```

**复制公钥到 GitHub:**

```bash
# macOS
pbcopy < ~/.ssh/id_ed25519.pub

# Linux
cat ~/.ssh/id_ed25519.pub
# 然后复制输出内容

# Windows (Git Bash)
cat ~/.ssh/id_ed25519.pub | clip
```

**在 GitHub 上添加 SSH 密钥:**

1. 点击右上角头像 → **Settings**
2. 左侧菜单选择 **SSH and GPG keys**
3. 点击 **New SSH key**
4. 输入标题，粘贴公钥
5. 点击 **Add SSH key**

**测试连接:**

```bash
ssh -T git@github.com
# 应该看到: Hi username! You've successfully authenticated...
```

### 配置 Git 用户信息

```bash
# 设置全局用户名和邮箱
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 查看配置
git config --list
```

---

## 仓库管理

### 创建新仓库

**方法 1: 在 GitHub 网站上创建**

1. 点击右上角 **+** → **New repository**
2. 输入仓库名称
3. 选择公开 (Public) 或私有 (Private)
4. 可选择添加 README、.gitignore、许可证
5. 点击 **Create repository**

**方法 2: 从本地创建**

```bash
# 在本地初始化仓库
git init
git add .
git commit -m "Initial commit"

# 在 GitHub 上创建空仓库后，连接并推送
git remote add origin git@github.com:username/repo-name.git
git branch -M main
git push -u origin main
```

### 克隆仓库

```bash
# 使用 HTTPS
git clone https://github.com/username/repo-name.git

# 使用 SSH (推荐)
git clone git@github.com:username/repo-name.git

# 克隆到指定目录
git clone git@github.com:username/repo-name.git my-project

# 克隆特定分支
git clone -b branch-name git@github.com:username/repo-name.git
```

### 仓库设置

**基本设置:**

- **Settings** → **General**: 修改仓库名称、描述、可见性
- **Settings** → **Branches**: 设置默认分支、分支保护规则
- **Settings** → **Collaborators**: 添加协作者
- **Settings** → **Pages**: 配置 GitHub Pages

**分支保护规则:**

1. 进入 **Settings** → **Branches**
2. 点击 **Add rule**
3. 设置分支名称模式 (如 `main` 或 `master`)
4. 启用保护选项:
   - ✅ Require pull request reviews
   - ✅ Require status checks
   - ✅ Require conversation resolution
   - ✅ Do not allow bypassing

### Fork 仓库

Fork 是创建仓库副本到你的账户，用于贡献开源项目。

**Fork 流程:**

```
原始仓库 ──▶ Fork ──▶ 你的账户
   │                      │
   │                      │ 修改代码
   │                      │
   └──────── Pull Request ─┘
```

**操作步骤:**

1. 在要 Fork 的仓库页面点击 **Fork**
2. 选择目标账户
3. Fork 完成后，克隆到本地:
   ```bash
   git clone git@github.com:your-username/repo-name.git
   ```
4. 添加上游仓库 (可选):
   ```bash
   git remote add upstream git@github.com:original-owner/repo-name.git
   ```

---

## 协作功能

### 添加协作者

**方法 1: 通过 Settings**

1. 进入仓库 **Settings** → **Collaborators**
2. 点击 **Add people**
3. 输入用户名或邮箱
4. 选择权限级别:
   - **Read**: 只能查看和克隆
   - **Write**: 可以推送代码
   - **Admin**: 完全管理权限

**方法 2: 通过组织 (Organization)**

组织可以更好地管理团队和权限。

### 权限级别

| 权限 | 说明 | 可执行操作 |
|------|------|-----------|
| Read | 只读 | 查看、克隆、下载 |
| Triage | 问题管理 | Read + 管理 Issues/PRs |
| Write | 写入 | Triage + 推送代码 |
| Maintain | 维护 | Write + 管理设置 (部分) |
| Admin | 管理员 | 完全控制 |

---

## Pull Request 工作流

### 什么是 Pull Request (PR)?

Pull Request 是请求将你的更改合并到主分支的机制。它是代码审查和讨论的平台。

```
你的分支 ──▶ 创建 PR ──▶ 代码审查 ──▶ 合并到主分支
   │              │          │
   │              │          └─ 讨论、修改
   │              └─ 描述更改
   └─ 推送代码
```

### 创建 Pull Request

**步骤 1: 准备分支**

```bash
# 从主分支创建功能分支
git checkout main
git pull origin main
git checkout -b feature/new-feature

# 进行开发并提交
git add .
git commit -m "feat: add new feature"
git push -u origin feature/new-feature
```

**步骤 2: 在 GitHub 上创建 PR**

1. 推送分支后，GitHub 会显示提示
2. 点击 **Compare & pull request**
3. 填写 PR 信息:
   - **标题**: 清晰描述更改
   - **描述**: 详细说明更改内容、原因、测试方法
   - **Reviewers**: 选择审查者
   - **Labels**: 添加标签
   - **Assignees**: 分配负责人
4. 点击 **Create pull request**

**PR 描述模板示例:**

```markdown
## 描述
简要描述这个 PR 做了什么

## 类型
- [ ] Bug 修复
- [ ] 新功能
- [ ] 文档更新
- [ ] 重构
- [ ] 其他

## 测试
- [ ] 已测试
- [ ] 测试通过

## 截图 (如适用)
<!-- 添加截图 -->

## 相关 Issue
Closes #123
```

### 审查 Pull Request

**审查操作:**

- ✅ **Approve**: 批准合并
- 💬 **Comment**: 添加评论
- ❌ **Request changes**: 请求修改
- 🔄 **Review suggestions**: 建议代码修改

**审查要点:**

1. 代码质量和风格
2. 功能正确性
3. 测试覆盖
4. 文档更新
5. 性能影响

### 更新 Pull Request

**添加新提交:**

```bash
# 在 PR 分支上继续开发
git checkout feature/new-feature
git add .
git commit -m "fix: address review comments"
git push
```

**修改之前的提交:**

```bash
# 修改最后一次提交
git commit --amend
git push --force-with-lease

# 或创建新提交
git add .
git commit -m "fix: update based on feedback"
git push
```

### 合并 Pull Request

**合并选项:**

1. **Create a merge commit**: 创建合并提交
   - 保留完整历史
   - 适合需要保留分支历史的场景

2. **Squash and merge**: 压缩合并
   - 将多个提交压缩为一个
   - 保持主分支历史简洁

3. **Rebase and merge**: 变基合并
   - 线性历史
   - 不创建合并提交

**合并后操作:**

```bash
# 切换到主分支
git checkout main

# 拉取最新更改
git pull origin main

# 删除本地分支
git branch -d feature/new-feature

# 删除远程分支 (如果已合并)
git push origin --delete feature/new-feature
```

---

## Issues 管理

### 创建 Issue

Issues 用于跟踪 bug、功能请求、任务等。

**创建步骤:**

1. 进入仓库 **Issues** 标签
2. 点击 **New issue**
3. 选择 Issue 类型:
   - **Bug report**: 报告 bug
   - **Feature request**: 功能请求
   - **Question**: 问题咨询
4. 填写标题和描述
5. 添加标签、分配负责人、关联项目
6. 点击 **Submit new issue**

**Issue 模板示例:**

```markdown
## Bug 描述
清晰描述 bug 是什么

## 重现步骤
1. 执行 '...'
2. 点击 '....'
3. 看到错误

## 预期行为
应该发生什么

## 实际行为
实际发生了什么

## 环境
- OS: [e.g. macOS 12.0]
- Browser: [e.g. Chrome 96]
- Version: [e.g. 1.2.3]

## 截图
如有截图，请添加

## 附加信息
其他相关信息
```

### Issue 标签

使用标签组织和管理 Issues:

| 标签类型 | 示例 | 用途 |
|---------|------|------|
| 类型 | `bug`, `feature`, `question` | 分类 Issue |
| 优先级 | `high`, `medium`, `low` | 优先级 |
| 状态 | `in-progress`, `blocked`, `needs-review` | 当前状态 |
| 领域 | `frontend`, `backend`, `docs` | 代码领域 |

### 关闭 Issue

**自动关闭:**

在提交信息或 PR 描述中使用关键词:

```bash
# 提交信息
git commit -m "fix: resolve login bug, closes #123"

# PR 描述
Closes #123
Fixes #456
Resolves #789
```

**手动关闭:**

1. 在 Issue 页面点击 **Close issue**
2. 添加关闭原因

---

## GitHub Actions

### 什么是 GitHub Actions?

GitHub Actions 是 CI/CD (持续集成/持续部署) 平台，可以自动化构建、测试、部署等工作流。

### 基本概念

```
事件触发 ──▶ 工作流运行 ──▶ 执行任务 ──▶ 报告结果
   │              │            │
   │              │            └─ 构建、测试、部署
   │              └─ 运行器 (Runner)
   └─ push, PR, issue
```

### 创建工作流

**文件位置:**

```
.github/
  └── workflows/
      └── ci.yml
```

**基本工作流示例:**

```yaml
name: CI

# 触发条件
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# 任务
jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
    
    - name: Install dependencies
      run: npm install
    
    - name: Run tests
      run: npm test
    
    - name: Build
      run: npm run build
```

### 常用 Actions

| Action | 用途 |
|--------|------|
| `actions/checkout@v3` | 检出代码 |
| `actions/setup-node@v3` | 设置 Node.js |
| `actions/setup-python@v4` | 设置 Python |
| `actions/cache@v3` | 缓存依赖 |
| `actions/upload-artifact@v3` | 上传构建产物 |

### 工作流示例

**Node.js 项目:**

```yaml
name: Node.js CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]
    
    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm ci
    - run: npm test
```

**Python 项目:**

```yaml
name: Python CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
        cache: 'pip'
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
    - name: Run tests
      run: pytest
```

---

## GitHub Pages

### 什么是 GitHub Pages?

GitHub Pages 是免费的静态网站托管服务，可以托管个人、项目或组织网站。

### 启用 GitHub Pages

**方法 1: 通过 Settings**

1. 进入仓库 **Settings** → **Pages**
2. 在 **Source** 选择分支和文件夹
3. 点击 **Save**
4. 网站将在 `https://username.github.io/repo-name` 可用

**方法 2: 使用 Actions**

创建 `.github/workflows/pages.yml`:

```yaml
name: Deploy to Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Setup Pages
      uses: actions/configure-pages@v3
    - name: Build
      run: npm run build
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v2
      with:
        path: './dist'
    - name: Deploy
      uses: actions/deploy-pages@v2
```

### 使用 Jekyll (默认)

GitHub Pages 默认支持 Jekyll，会自动构建 Markdown 和 HTML。

**创建 `_config.yml`:**

```yaml
title: My Awesome Site
description: A simple site
theme: minima
```

### 自定义域名

1. 在仓库根目录创建 `CNAME` 文件:
   ```
   example.com
   ```
2. 在 DNS 提供商添加记录:
   - Type: `CNAME`
   - Name: `@` 或 `www`
   - Value: `username.github.io`
3. 在 GitHub Pages 设置中启用 **Enforce HTTPS**

---

## GitHub CLI

### 安装 GitHub CLI

**macOS:**
```bash
brew install gh
```

**Windows:**
```bash
winget install GitHub.cli
```

**Linux:**
```bash
# Ubuntu/Debian
sudo apt install gh

# Fedora
sudo dnf install gh
```

### 认证

```bash
# 登录
gh auth login

# 选择认证方式
# - GitHub.com
# - HTTPS 或 SSH
# - 浏览器登录或 token

# 查看认证状态
gh auth status

# 登出
gh auth logout
```

### 常用命令

**仓库操作:**

```bash
# 克隆仓库
gh repo clone username/repo-name

# 创建仓库
gh repo create my-project --public
gh repo create my-project --private

# 查看仓库信息
gh repo view

# Fork 仓库
gh repo fork username/repo-name

# 列出仓库
gh repo list
```

**Pull Request:**

```bash
# 创建 PR
gh pr create --title "Add feature" --body "Description"

# 查看 PR 列表
gh pr list

# 查看 PR 详情
gh pr view 123

# 合并 PR
gh pr merge 123

# 关闭 PR
gh pr close 123
```

**Issues:**

```bash
# 创建 Issue
gh issue create --title "Bug" --body "Description"

# 查看 Issues
gh issue list

# 查看 Issue 详情
gh issue view 123

# 关闭 Issue
gh issue close 123
```

**工作流:**

```bash
# 查看工作流
gh workflow list

# 运行工作流
gh workflow run workflow-name.yml

# 查看运行历史
gh run list

# 查看运行详情
gh run view 123
```

---

## 最佳实践

### ✅ 推荐做法

1. **清晰的提交信息**: 使用规范的提交信息格式
2. **小步提交**: 每次提交只做一件事
3. **使用分支**: 新功能在独立分支开发
4. **代码审查**: 所有代码通过 PR 审查
5. **Issue 跟踪**: 使用 Issues 跟踪任务和 bug
6. **README 文档**: 编写清晰的 README
7. **许可证**: 选择合适的开源许可证
8. **CI/CD**: 使用 Actions 自动化测试和部署

### ❌ 避免做法

1. ❌ 不要提交敏感信息 (密码、API 密钥)
2. ❌ 不要强制推送到主分支
3. ❌ 不要在 PR 中提交大量无关更改
4. ❌ 不要忽略代码审查反馈
5. ❌ 不要使用模糊的 Issue/PR 标题
6. ❌ 不要提交大文件 (使用 Git LFS)

### README 最佳实践

**好的 README 应包含:**

```markdown
# 项目名称

简短的项目描述

## 功能特性

- 功能 1
- 功能 2

## 安装

\`\`\`bash
npm install
\`\`\`

## 使用方法

\`\`\`bash
npm start
\`\`\`

## 贡献

欢迎贡献！请阅读 CONTRIBUTING.md

## 许可证

MIT License
```

### .gitignore 检查

确保 `.gitignore` 包含:

```gitignore
# 环境变量
.env
.env.local

# 依赖
node_modules/
vendor/

# 构建产物
dist/
build/

# 日志
*.log

# IDE
.vscode/
.idea/

# 系统文件
.DS_Store
Thumbs.db
```

---

## 🎯 快速参考卡片

### 日常协作流程

```bash
# 1. 更新主分支
git checkout main
git pull origin main

# 2. 创建功能分支
git checkout -b feature/new-feature

# 3. 开发并提交
git add .
git commit -m "feat: add feature"
git push -u origin feature/new-feature

# 4. 在 GitHub 创建 PR

# 5. 合并后清理
git checkout main
git pull origin main
git branch -d feature/new-feature
```

### GitHub CLI 常用命令

```bash
gh repo clone user/repo    # 克隆仓库
gh pr create               # 创建 PR
gh pr list                 # 查看 PR
gh issue create            # 创建 Issue
gh workflow run name       # 运行工作流
```

### Pull Request 检查清单

- [ ] 代码已测试
- [ ] 通过所有 CI 检查
- [ ] 更新了文档
- [ ] 添加了测试
- [ ] 提交信息清晰
- [ ] PR 描述完整
- [ ] 解决了相关 Issue

---

## 📖 学习资源

- [GitHub 官方文档](https://docs.github.com/)
- [GitHub Skills](https://skills.github.com/)
- [GitHub Guides](https://guides.github.com/)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [GitHub CLI 文档](https://cli.github.com/manual/)

---

## 🔗 相关资源

- [Git 学习指南](./learn_git_guide.md) - 掌握 Git 基础

---

> 💡 **提示**: GitHub 是一个强大的协作平台。多实践、多参与开源项目，是学习的最佳方式！

---

*Created with ❤️ for GitHub learners*
