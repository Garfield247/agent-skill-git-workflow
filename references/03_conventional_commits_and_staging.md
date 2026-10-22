# Conventional Commits 规范与精准暂存 (Commits & Staging)

## 1. 提交信息格式规范

统一遵循 **Conventional Commits** 语义化规范，采用简体中文动宾描述：

```text
<type>(<scope>): <中文动宾描述>

[可选详细说明]
```

### 常用 Type 类型字典
| Type | 语义 | 适用场景 |
| :--- | :--- | :--- |
| **`feat`** | 新功能 | 新增 API 接口、实现导出、增加微信登录 |
| **`fix`** | Bug 修复 | 修复空指针 Panic、修复金额计算精度 |
| **`refactor`**| 代码重构 | 提取公共函数、调整类结构（非新功能也无 Bug） |
| **`perf`** | 性能优化 | 增加 Redis 缓存、优化慢 SQL 索引 |
| **`docs`** | 文档变更 | 补充接口契约、更新架构图、修改 README |
| **`test`** | 测试用例 | 增加单元测试、集成测试 |
| **`chore`** | 构建/杂项 | 更新依赖、修改 .gitignore、调整 CI 脚本 |

---

## 2. 精准暂存流 (Precise Staging)

- **严禁盲目执行 `git add .`**：避免误将编译二进制、环境密码文件、临时日志或开发半成品带入版本库；
- **强制显式精准暂存**：
  ```bash
  git add internal/logic/order_logic.go project_wiki/02_架构设计/order_state.md
  git commit -m "feat(order): 增加订单取消时自动回退库存逻辑"
  ```
- **自动生成物同频提交**：契约变更重新生成代码后，手写 Logic 必须与生成的 Handler/Types 打包在同一个原子 Commit 中，确保每个提交节点均能独立编译通过。

---

## 3. 未推送提交原子追加与修补规范 (Unpushed Commit Amendment)

> 💡 **核心目标**：彻底消灭无意义的微小碎提交（如“改错字”、“补参数”），保持线性提交历史的高信息密度与原子自洽性。

### 3.1 状态判定与决策分支 (Decision Matrix)
- 🟢 **未推送状态 (`ahead of origin` / 纯本地提交)**：
  - **决策**：**强力推荐优先追加修补（Amend）**，将补充改动合并至上一个原子 Commit 中；
- 🔴 **已推送状态 (`up to date with origin` / 公共分支已发布)**：
  - **决策**：**坚决禁止 `--amend`**！防止本地与远程基准分叉导致强推覆写他人代码；必须新开一个独立的语义化补丁提交（如 `fix(scope): ...`）。

### 3.2 标准原子追加命令范式
```bash
# 1. 显式精准暂存修补文件
git add internal/logic/order_logic.go

# 2. 推荐：无声追加（保持原提交说明与 Type 格式不变）
git commit --amend --no-edit

# 3. 备选：若微调涉及提交说明补充，可显式覆写说明
git commit --amend -m "feat(order): 增加订单取消时自动回退库存并补全幂等拦截"
```

