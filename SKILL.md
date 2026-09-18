---
name: git-workflow-mastery
description: >-
  Git 全生命周期研发工作流、分支治理、变基冲突安全化解、Conventional Commits 原子提交与 .gitignore 分类治理生产级规范技能。
  覆盖日常分支协作 (feature/bugfix/hotfix)、暂存与恢复 (stash/restore)、变基与冲突分步化解、
  完成即提交 (Conventional Commits)、.gitignore 动态四大类维护以及防破坏性强制推送 (Safety Redlines)。
---

# Git 全场景日常操作与提交治理规范技能 (Git Workflow Mastery Skill)

## 概述 (Overview)

本技能定义了研发工程师与 AI 编码助手在进行版本控制、分支协作、代码暂存、合并冲突化解、规范化原子提交（Conventional Commits）以及 `.gitignore` 动态治理时的通用工业级标准与安全操作准则。

### 核心设计原则

1. **原子性提交 (Atomic Commits)**：每个 Commit 仅包含一个逻辑独立的变更单元（单一功能特性 / 单一 Bug 修复），严禁将多个不相关的任务混入同一个提交。
2. **Conventional Commits 语义化规范**：统一使用 `<type>(<scope>): <subject>` 格式，类型明确，中文描述简洁。
3. **主干线性与变基优先 (Rebase over Merge)**：同步远程最新代码提倡使用 `git pull --rebase`，保持提交图谱的清晰线性，减少无意义的分支分叉与合并气泡。
4. **防御性安全红线 (Zero-Destructive Guardrails)**：绝对禁止未经用户明确授权执行 `git reset --hard`、`git clean -fd` 或 `git push -f` 强推操作，杜绝代码与未暂存数据丢失。
5. **.gitignore 动态分类分块维护**：自动感知环境配置文件、编译输出物、依赖目录与临时日志，分门别类维护并保持清晰的注释。

---

# 1. 分支治理与日常操作规范 (Branching Strategy)

## 1.1 分支命名规范
全项目分支统一遵循语义化中划线命名：
- **功能特性分支**：`feature/{issue_id}-{brief_desc}` 或 `feature/{username}-{feature_name}`（例如 `feature/102-user-auth`、`feature/order-export`）
- **Bug 修复分支**：`bugfix/{issue_id}-{bug_desc}`（例如 `bugfix/98-nil-pointer-panic`）
- **紧急线上热修复**：`hotfix/{issue_id}-{cve_or_bug}`（基于主分支创建，合入后打 Tag）
- **主干与稳定分支**：`main` / `master`（受保护主干，仅允许通过 PR/MR 合入并自动触发 CI/CD）

## 1.2 分支日常高频命令
```bash
# 1. 基于最新主干创建并切换到特性分支
git checkout main
git pull --rebase origin main
git checkout -b feature/order-payment

# 2. 查看当前分支及本地/远程跟踪状态
git branch -vv

# 3. 清理已合并的本地陈旧分支
git branch --merged main | grep -v '^\*' | grep -v 'main' | xargs -n 1 git branch -d
```

---

# 2. 工作区暂存、对比与撤销恢复 (Working Tree & Stash)

## 2.1 修改前后的状态与差异自检
```bash
# 修改前：简要自检工作区干净程度
git status --short

# 精准查看已暂存区与上次提交的差异
git diff --staged

# 精准查看工作区尚未暂存的修改行数统计与详情
git diff --stat
git diff
```

## 2.2 工作区临时变更暂存 (`git stash`)
在需要临时切换分支处理紧急 Bug 或拉取最新代码时：
```bash
# 1. 显式添加备注进行暂存 (严禁无备注裸跑 git stash)
git stash push -m "wip: 正在调试订单支付逻辑，临时暂存"

# 2. 查看暂存列表
git stash list

# 3. 恢复最近一次暂存并保留 stash 栈记录 (安全推荐)
git stash apply stash@{0}

# 4. 彻底弹出并移除最近一次暂存
git stash pop
```

## 2.3 安全撤销与恢复指南 (Undo & Restore)
- **撤销工作区单个文件的未暂存修改**：
  ```bash
  # 恢复单个文件到暂存区/最新 commit 状态 (丢弃未暂存的手工修改，需审慎)
  git restore <file_path>
  ```
- **将已暂存的文件撤回至未暂存状态**：
  ```bash
  # 移出暂存区，保留工作区代码不变
  git restore --staged <file_path>
  ```
- **撤回最近一次本地提交，但保留修改代码在暂存区 (Commit 修正神技)**：
  ```bash
  # 撤销上次 commit，代码全部退回暂存区，可直接修改后重新 commit
  git reset --soft HEAD~1
  ```
- **灾难性恢复 (找回丢失的 Commit 或误删的分支)**：
  ```bash
  # 查看所有操作指针历史
  git reflog
  # 恢复到指定的变更点
  git checkout -b recovery-branch <commit_hash>
  ```

---

# 3. 变基同步与合并冲突化解 (Rebase & Conflicts)

## 3.1 保持线性历史：变基同步主干
提倡使用 `rebase` 替代 `merge`，消除无效的 `Merge branch 'main' into ...` 提交噪音：
```bash
# 获取远程最新提交
git fetch origin

# 将当前特性分支基于主干最新代码变基
git rebase origin/main
```

## 3.2 冲突安全解决标准流程
当 Rebase 或 Merge 产生冲突时，必须按以下 4 步规范化执行：

```text
1. 运行 git status 查看所有标记为 Both Modified 的冲突文件
2. 逐一打开文件，人工核对 <<<<<<< HEAD 与 >>>>>>> 标记段落
3. 结合业务意图保留正确代码，删除冲突标记符并保存
4. 运行 git add <resolved_file> 标记冲突已解决
5. 运行 git rebase --continue 完成剩余变基步骤
```

- **放弃变基（遇不可控异常时的撤销手段）**：
  ```bash
  git rebase --abort
  ```

---

# 4. 规范化原子提交 (Conventional Commits)

## 4.1 完成即提交原则 (Done Means Commit)
每当一个独立的需求功能点或 Bug 修复实现并本地验证通过后，**必须主动提议并执行本地 `git commit`**，无需等待人类多次提醒。

## 4.2 提交信息格式 (Commit Message Specification)
统一遵循 **Conventional Commits** 标准：

```text
<type>(<scope>): <subject>

[可选 body 详细描述]
[可选 footer 关联 issue/任务]
```

### 常用 Type 类型字典
| Type | 语义 | 适用场景与范例 |
| :--- | :--- | :--- |
| **`feat`** | 新功能 (Feature) | 新增用户登录、扩展 API 接口、实现导出功能 |
| **`fix`** | Bug 修复 | 修复空指针 Panic、修复金额计算精度问题 |
| **`refactor`**| 代码重构 | 调整类结构、提炼公共函数（既非新功能也无 Bug 修复） |
| **`perf`** | 性能优化 | 增加 Redis 缓存、优化 SQL 查询索引减少时延 |
| **`docs`** | 文档变更 | 补充接口文档、修改 README、补充架构图表 |
| **`test`** | 测试用例 | 新增单元测试、集成测试用例、补充 Mock 数据 |
| **`chore`** | 构建/依赖/杂项 | 更新 go.mod / package.json 依赖、维护 .gitignore、调整 CI 脚本 |
| **`style`** | 格式调整 | 代码格式化、修复换行缩进（不影响代码运行逻辑） |

### Scope 范围标注与 Subject 规范
- **Scope 范围**：用括号指定变更的业务模块或目录（如 `user`, `order`, `auth`, `model`, `api`）；
- **Subject 描述**：
  - 使用简体中文，简洁直指变更核心；
  - 动宾结构（如 `feat(order): 增加微信支付回调超时处理`）；
  - 结尾不加句号。

## 4.3 精准暂存流 (Precise Staging)
- **严禁盲目执行 `git add .`**：避免误将编译二进制、环境密码文件、临时日志或开发中的半成品带入版本库；
- **强制执行显式文件暂存**：
  ```bash
  git add internal/logic/order_logic.go internal/types/types.go
  git commit -m "feat(order): 增加订单取消时自动回退库存逻辑"
  ```
- **代码生成物原子提交**：若修改了契约文件导致框架重新生成了代码（如 goctl 的 handler 与 types），**必须将手写 Logic 与生成的 Handler/Types 打包在同一个原子 Commit 中**，保证任意历史 Commit 均能独立编译通过。

---

# 5. `.gitignore` 动态感知与四大分类治理

在研发过程中，若产生新的编译产物、依赖目录、日志、临时文件或环境配置文件，必须主动检查并在项目根目录的 `.gitignore` 中追加对应的忽略规则。

## 5.1 四大模块分类标准模板
所有 `.gitignore` 文件统一采用以下 4 个标准分块组织并附带中文注释：

```gitignore
# ====================================================
# 1. Environment & Secrets (环境与敏感凭证)
# ====================================================
.env
.env.*
!.env.example
*.pem
*.key
id_rsa*
oauth_creds.json

# ====================================================
# 2. Build Outputs & Binaries (构建产物与二进制可执行文件)
# ====================================================
/bin/
/dist/
/out/
*.exe
*.exe~
*.dll
*.so
*.dylib
*.test
*.out

# ====================================================
# 3. Dependencies & Package Managers (依赖包目录)
# ====================================================
vendor/
node_modules/
.pnpm-store/
__pycache__/
*.py[cod]
*$py.class
.venv/
env/

# ====================================================
# 4. Logs, IDEs & OS Temp (日志、编辑器与系统临时缓存)
# ====================================================
*.log
logs/
.DS_Store
Thumbs.db
.idea/
.vscode/
*.swp
*.swo
.cache/
.scratch/
```

---

# 6. 防御性安全操作红线 (Safety Guardrails)

1. **严禁静默强制覆写 (No Silent Push -f)**：
   - 严禁在公共主干（`main`, `master`, `dev`, `release`）执行 `git push --force`；
   - 个人特性分支若因 Rebase 必须强推，必须使用安全的 `git push --force-with-lease`。
2. **严禁硬重置工作区 (No Destructive Reset/Clean)**：
   - 绝对禁止未经用户明确确认执行 `git reset --hard`；
   - 绝对禁止未经用户明确确认执行 `git clean -fd` 抹除未跟踪文件。
3. **敏感凭证防泄露**：
   - 提交前检查暂存区，严禁将真实的密码、生产 API 密钥、数据库连接字符串提交到 Git 仓库。

---

# 6. Git 异常状态与冲突急救指引 (Git Troubleshooting & Recovery)

在日常协作中遇到冲突或异常 Git 状态时，必须遵循以下安全指引：

### 6.1 变基冲突与中止 (Rebase Conflict Recovery)
- **现象**：终端提示 `CONFLICT (content): Merge conflict in ...`，处于 `(rebase 1/3)` 状态；
- **排查与处置**：
  1. 运行 `git status` 确认冲突文件清单；
  2. 若现场过于复杂希望彻底恢复原状，执行安全中止命令：
     ```bash
     git rebase --abort
     ```
  3. 若手工化解冲突后，依次暂存并继续：
     ```bash
     git add <resolved-files>
     git rebase --continue
     ```
  - **绝对红线**：严禁在未化解冲突的情况下无脑运行 `git rebase --skip`，避免丢失关键提交！

### 6.2 黄金逃生门：Reflog 撤销与误操作自愈
- **误执行了 `git reset --hard` 或误删分支**：
  Git 的每一次引用变更（Commit、Checkout、Reset）均在本地 reflog 留存：
  ```bash
  git reflog
  # 找到操作前的 HEAD 编号（如 HEAD@{2}）并安全跳回：
  git reset --hard HEAD@{2}
  ```

### 6.3 游离头指针状态 (Detached HEAD State)
- **现象**：终端提示 `HEAD detached at <commit>`，在此提交的代码可能在切换分支后被当做垃圾回收；
- **处置**：基于当前提交立即创建新分支保存成果：
  ```bash
  git branch fix/recovered-work
  git checkout fix/recovered-work
  ```
