# 防御性安全红线与 Reflog 逃生门 (Safety Guardrails & Reflog)

## 1. 生产级安全操作三大红线

1. 🚨 **严禁在共享主干盲目强制推送 (`git push -f`)**：
   - 特性分支变基后推送，必须使用具备租约保护的命令：
     ```bash
     git push --force-with-lease origin <branch-name>
     ```
   - 若远程已被队友更新，该命令自动安全拦截，防止误杀他人代码。
2. 🚨 **严禁未经授权执行破坏性抹除**：
   - 坚决杜绝未经用户明确确认执行 `git reset --hard`；
   - 坚决杜绝未经用户明确确认执行 `git clean -fd` 清除未跟踪文件。
3. 🚨 **敏感凭证防泄漏预检**：
   - 提交前检查暂存区，严禁包含真实 API Key、私钥证书或数据库密码。

---

## 2. 黄金逃生门：Reflog 撤销与误操作自愈

当误执行了 `git reset --hard` 或误删分支时，通过引用日志找回历史变更点：

```bash
git reflog
# 找到操作前的 HEAD 编号并恢复：
git reset --hard HEAD@{1}
```
