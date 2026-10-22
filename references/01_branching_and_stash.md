# 分支治理策略与工作区暂存 (Branching & Stash)

## 1. 分支命名规范

- **功能特性分支**：`feature/{issue_id}-{brief_desc}`（例如 `feature/102-user-auth`）；
- **Bug 修复分支**：`bugfix/{issue_id}-{bug_desc}`（例如 `bugfix/98-nil-pointer-panic`）；
- **线上热修复**：`hotfix/{issue_id}-{cve_or_bug}`（基于主干创建，合入后打 Tag）；
- **受保护主干**：`main` / `master`（仅允许通过 PR/MR 合入，禁止直接在主干工作）。

---

## 2. 工作区临时暂存 (`git stash`)

需要临时切换分支处理紧急 Bug 或拉取最新代码时：

```bash
# 1. 显式添加备注进行暂存 (严禁无备注裸跑 git stash)
git stash push -m "wip: 正在调试订单支付逻辑，临时暂存"

# 2. 查看暂存列表
git stash list

# 3. 恢复最近一次暂存并保留 stash 栈记录 (安全推荐)
git stash apply stash@{0}

# 4. 弹出并彻底移除最近一次暂存
git stash pop
```
