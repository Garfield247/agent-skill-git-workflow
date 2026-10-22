---
name: git-workflow-mastery
description: >-
  Git 全生命周期研发工作流、分支治理、变基冲突安全化解、Conventional Commits 原子提交与 .gitignore 分类治理生产级规范技能。
  覆盖日常分支协作 (feature/bugfix/hotfix)、暂存与恢复 (stash/restore)、变基与冲突分步化解、
  完成即提交 (Conventional Commits)、.gitignore 动态四大类维护以及防破坏性强制推送 (Safety Redlines)。
---

# Git 全场景日常操作与提交治理规范 (Git Workflow Mastery Skill)

本技能定义了进行分支治理、变基同步、代码暂存、Conventional Commits 原子提交与安全推送时的工程标准与渐进式手册索引。

---

## 1. 核心铁律与绝对红线 (Core Redlines)

1. 🚨 **完成即提交 (Done Means Commit Gate)**：
   - 任务功能完成且自动化测试/编译通过后，**必须在当前会话轮次主动执行本地 Git 提交闭环**，严禁修改后裸奔交付。
2. 🚨 **精准显式暂存 (Precise Staging)**：
   - **坚决禁止无脑 `git add .`**；必须显式指定文件（`git add <file1> <file2>`），防止临时日志、配置或二进制意外进仓。
3. 🚨 **Conventional Commits 语义化规范**：
   - 统一使用 `<type>(<scope>): <中文动宾描述>`（例如 `feat(auth): 增加微信登录回调校验`），严禁无意义提交。
4. 🚨 **防破坏性强推与抹除禁令**：
   - **绝对禁止使用裸 `git push -f`**，必须使用带有租约锁的 `git push --force-with-lease`；
   - 绝对禁止未经用户明确授权执行 `git reset --hard` 或 `git clean -fd`。

---

## 2. 标准开发工作流 (Standard Workflow)

1. **变基拉取**：从主干拉取最新代码采用 `git pull --rebase origin main`；
2. **特性开发**：基于语义化分支（`feature/`、`bugfix/`）独立演进；
3. **差异自检**：运行 `git diff --staged` 与 `git status -s` 核对真实变更；
4. **精准暂存**：将业务代码与架构文档同步精准 `git add`；
5. **原子交付**：输出 Short Commit Hash 与中文动宾描述。

---

## 3. 渐进式专业参考手册 (Progressive Disclosure References)

根据具体涉及的业务场景，**按需查阅 `references/` 目录下的专门规约**，严禁将全量子文档一次性无脑全部展开：

| 场景模块 | 参考子文档 | 核心涵盖规范 |
| :--- | :--- | :--- |
| **分支策略与暂存** | [01_branching_and_stash.md](references/01_branching_and_stash.md) | • feature/bugfix/hotfix 分支命名字典<br>• `git stash push -m` 备注暂存与安全恢复 |
| **变基与冲突化解** | [02_rebase_and_conflict_resolution.md](references/02_rebase_and_conflict_resolution.md) | • 线性历史变基同步主干流程<br>• 冲突 4 步标准化化解与 `rebase --abort` 逃生门 |
| **原子提交与精准暂存**| [03_conventional_commits_and_staging.md](references/03_conventional_commits_and_staging.md) | • Conventional Commits 格式与常用 Type 字典<br>• 显式精准暂存与未推送提交原子追加 (--amend --no-edit) |
| **.gitignore 四大分类** | [04_gitignore_four_tier_template.md](references/04_gitignore_four_tier_template.md) | • 凭证、构建产物、依赖包与临时日志四大分块<br>• 动态感知与标准模板维护 |
| **安全红线与 Reflog** | [05_safety_guardrails_and_reflog.md](references/05_safety_guardrails_and_reflog.md) | • `--force-with-lease` 租约推送防覆写红线<br>• `git reflog` 撤销误操作自愈急救 |
