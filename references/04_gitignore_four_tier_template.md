# .gitignore 动态感知四大分类标准模板 (Gitignore Standards)

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
.cache/
.scratch/
```
