# Git Workflow Mastery Skill (Git 全场景工程工作流与提交治理规范)

[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Git Version](https://img.shields.io/badge/git-2.30%2B-brightgreen.svg)](https://git-scm.com)
[![Target Agents](https://img.shields.io/badge/Agents-Antigravity%20%7C%20Claude%20Code%20%7C%20Cursor%20%7C%20Codex-purple.svg)](#)

本仓库提供了一套面向现代研发团队与 AI 编码助手的 **Git 全场景日常操作与提交治理规范技能**。涵盖了分支协作治理、日常暂存与恢复、线性变基与冲突化解、Conventional Commits 原子提交以及 `.gitignore` 四大分类模块化动态治理。

---

## 🌟 核心规范亮点 (Highlights)

- **分支全流程协作**：规范 `feature/*`、`bugfix/*`、`hotfix/*` 命名语义与生命周期清理。
- **暂存与安全撤销**：规范 `git stash push -m` 语义化暂存、`git restore` 工作区保护、`git reset --soft` 提交重排与 `git reflog` 灾难性找回。
- **变基优先与冲突化解**：提倡 `git pull --rebase` 保持提交图谱线性，提供标准的 5 步冲突安全化解流程。
- **完成即提交 (Conventional Commits)**：单一特性/Bug 修复验证后主动触发原子提交，规范 8 大标准 Type 标签。
- **.gitignore 动态四大类治理**：按 Environment / Build / Dependencies / Logs & Temp 四大区块统一维护，保持代码库极简整洁。
- **防御性操作红线**：严禁静默 `reset --hard`、`clean -fd` 或对主干 `push -f`。

---

## 📂 仓库结构 (Repository Layout)

```text
.
├── SKILL.md                          # 根目录 Agent Skill 核心规范说明文件
├── skills/
│   └── git-workflow-mastery/
│       └── SKILL.md                  # 符合标准多 Skill 管理器规范的目录结构
├── .gitignore                        # Git 忽略规则
├── LICENSE                           # MIT 开源许可证
└── README.md                         # 详尽的中文项目说明与章节导航
```

---

## 🚀 安装与使用 (Installation & Usage)

### 1. Antigravity IDE / Antigravity CLI
全局自动生效：
```bash
mkdir -p ~/.gemini/config/skills/git-workflow-mastery
cp SKILL.md ~/.gemini/config/skills/git-workflow-mastery/SKILL.md
```

作为项目工作区本地技能生效：
```bash
mkdir -p .agents/skills/git-workflow-mastery
cp SKILL.md .agents/skills/git-workflow-mastery/SKILL.md
```

### 2. 通过 Agent Skills 包管理器安装
```bash
npx skills add Garfield247/agent-skill-git-workflow
```

---

## 📄 开源许可证 (License)

本项目基于 [MIT License](LICENSE) 开源。
