# 变基同步与冲突化解流程 (Rebase & Conflict Resolution)

## 1. 变基同步主干 (保持历史线性)

提倡使用 `rebase` 替代 `merge`，消除无效的 `Merge branch 'main' into ...` 提交气泡：

```bash
git fetch origin
git rebase origin/main
```

---

## 2. 冲突化解 4 步标准化流程

当 Rebase 产生冲突处于 `(rebase 1/N)` 状态时：

```text
1. 运行 git status 查看标记为 Both Modified 的冲突文件列表
2. 逐一打开文件，核对 <<<<<<< HEAD 与 >>>>>>> 冲突段落
3. 结合业务意图保留正确代码，删除冲突标记符并保存
4. 运行 git add <resolved_file> 标记冲突已解决
5. 运行 git rebase --continue 完成剩余步骤
```

- **放弃变基（遇不可控异常时的撤销手段）**：
  ```bash
  git rebase --abort
  ```
  *(严禁在未解决冲突时无脑运行 `git rebase --skip`，避免丢失关键提交！)*
