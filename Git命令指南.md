# AI 辅助 Git 代码管理指南

> 使用 AI 生成代码时的 Git 常用命令和最佳实践

---

## 目录

- [基础操作](#基础操作)
- [分支管理](#分支管理)
- [提交和推送](#提交和推送)
- [回退和撤销](#回退和撤销)
- [查看历史](#查看历史)
- [让 AI 管理 Git](#让-ai-管理-git)
- [常用场景](#常用场景)

---

## 基础操作

### 初始化仓库
```bash
git init
```

### 添加远程仓库
```bash
git remote add origin <仓库URL>
```

### 克隆仓库
```bash
git clone <仓库URL>
```

### 查看状态
```bash
git status
```

---

## 分支管理

### 查看分支
```bash
git branch          # 查看本地分支
git branch -a       # 查看所有分支（包括远程）
```

### 创建分支
```bash
git branch <分支名>
```

### 切换分支
```bash
git checkout <分支名>
git switch <分支名>   # 新语法
```

### 创建并切换分支
```bash
git checkout -b <分支名>
git switch -c <分支名>  # 新语法
```

### 合并分支
```bash
git checkout main        # 先切换到主分支
git merge <要合并的分支>
```

### 删除分支
```bash
git branch -d <分支名>    # 安全删除（已合并）
git branch -D <分支名>    # 强制删除
```

---

## 提交和推送

### 添加到暂存区
```bash
git add <文件名>           # 添加指定文件
git add .                  # 添加所有修改
git add -A                 # 添加所有修改（包括删除）
```

### 提交
```bash
git commit -m "提交说明"
```

### 推送到远程
```bash
git push                    # 推送到当前分支
git push origin main        # 推送到指定分支
git push -u origin main     # 推送并设置上游分支
```

### 拉取更新
```bash
git pull                    # 拉取并合并
git pull --rebase           # 拉取并变基
```

---

## 回退和撤销

### 撤销工作区修改
```bash
git restore <文件名>        # 撤销未暂存的修改
git checkout -- <文件名>    # 旧语法
```

### 撤销暂存
```bash
git restore --staged <文件名>
git reset HEAD <文件名>     # 旧语法
```

### 撤销提交（保留修改）
```bash
git reset --soft HEAD~1     # 撤销最近1次提交，修改保留在暂存区
git reset --mixed HEAD~1    # 撤销最近1次提交，修改保留在工作区
```

### 撤销提交（丢弃修改）
```bash
git reset --hard HEAD~1     # ⚠️ 完全丢弃，不可恢复
```

### 回退到指定提交
```bash
git reset --hard <commit_id>     # 本地回退
git push --force origin main     # ⚠️ 强制推送（改写历史）
```

### 撤销已推送的提交（安全方式）
```bash
git revert <commit_id>      # 创建新提交来撤销
```

---

## 查看历史

### 查看提交日志
```bash
git log                    # 完整日志
git log --oneline          # 简洁日志
git log --graph            # 图形化显示分支
```

### 查看操作历史（找回误删的提交）
```bash
git reflog                 # 查看所有操作记录
```

### 对比差异
```bash
git diff                   # 工作区 vs 暂存区
git diff --staged          # 暂存区 vs 最后提交
git diff <commit1> <commit2>  # 对比两个提交
```

---

## 让 AI 管理 Git

### 推荐的 AI 提示词

#### 1. 初始化项目
```
帮我初始化这个文件夹为 git 项目，并推送到 GitHub
```

#### 2. 创建分支开发
```
创建一个新分支 feature/xxx，在这个分支上开发
```

#### 3. 提交代码
```
帮我把修改提交到 git，提交信息写"xxx"
```

#### 4. 合并分支
```
把 feature/xxx 分支合并到 main，然后删除这个分支
```

#### 5. 回退代码
```
帮我回退到 xxx 提交，后面的提交都不要了
```

#### 6. 查看历史
```
帮我看看最近的提交历史
```

#### 7. 比较差异
```
帮我比较这两个提交有什么不同
```

### AI 会帮你做的事

| 操作 | AI 命令示例 |
|------|------------|
| 初始化 | `git init` |
| 提交 | `git add . && git commit -m "xxx"` |
| 推送 | `git push -u origin main` |
| 合并 | `git checkout main && git merge feature` |
| 回退 | `git reset --hard <commit_id>` |
| 删除分支 | `git branch -D <分支名>` |

---

## 常用场景

### 场景1：新功能开发
```bash
# 1. 创建功能分支
git checkout -b feature/new-feature

# 2. 开发并提交
git add .
git commit -m "实现新功能"

# 3. 合并回主分支
git checkout main
git merge feature/new-feature
git branch -d feature/new-feature
git push
```

### 场景2：代码回退
```bash
# 查看历史找到要回退的提交
git log --oneline

# 回退到指定提交
git reset --hard <commit_id>

# 强制推送
git push --force origin main
```

### 场景3：找回误删的提交
```bash
# 查看操作历史
git reflog

# 找到要恢复的 commit_id
git reset --hard <commit_id>
```

---

## 注意事项

### ⚠️ 危险操作
- `git reset --hard` - 会丢弃所有修改
- `git push --force` - 会覆盖远程历史

### ✅ 安全习惯
1. 修改前先创建分支
2. 重要修改及时提交
3. 使用 `git revert` 代替 `git reset`（已推送的代码）
4. 定期推送到远程备份

### 📝 提交规范
- 提交信息要清晰描述做了什么
- 示例：
  - ✅ "新增用户登录功能"
  - ✅ "修复支付金额计算错误"
  - ❌ "update"
  - ❌ "fix bug"

---

## 快速参考表

| 想做什么 | 命令 |
|---------|------|
| 查看状态 | `git status` |
| 添加所有修改 | `git add .` |
| 提交 | `git commit -m "说明"` |
| 推送 | `git push` |
| 拉取 | `git pull` |
| 创建分支 | `git branch <名>` |
| 切换分支 | `git checkout <名>` |
| 合并分支 | `git merge <分支名>` |
| 删除分支 | `git branch -D <名>` |
| 查看历史 | `git log --oneline` |
| 回退提交 | `git reset --hard <id>` |
| 撤销修改 | `git restore <文件名>` |

---

*最后更新: 2026-06-21*
