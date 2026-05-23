# Coder Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/Version-2.1.0-green.svg)](SKILL.md)
[![Coder Skills](https://img.shields.io/badge/Coder_Skills-18-orange.svg)](#)
[![Auto Route](https://img.shields.io/badge/Auto_Route-Enabled-blue.svg)](#)

开发者技能索引路由器 - 接收任何开发任务，智能推荐最合适的 skill 并执行。

## 一句话路由规则

```
收到 Coder 任务
    │
    ├─ "开发/新功能" → bmad-dev
    ├─ "bug/调试" → debugging-strategies
    ├─ "测试" → write-tests / tdd
    ├─ "分支/PR" → finishing-a-development-branch
    ├─ "重构" → refactor-code
    └─ 无匹配 → bmad-dev
```

## 目录

- [快速开始](#快速开始)
- [技能地图](#技能地图)
- [工作流](#工作流)
- [升级](#升级)

## 快速开始

```bash
# 安装
git clone https://gitee.com/ztanfo_admin/coder-ext-skill.git ~/.hermes/profiles/coder/skills/coder-ext-skill

# 使用
hermes -p coder -s coder-ext-skill
```

## 技能地图

| Skill | 说明 | 级别 |
|-------|------|------|
| bmad-dev | 开发规范核心 | P0 |
| debugging-strategies | 调试策略 | P0 |
| error-handling-patterns | 错误处理 | P0 |
| write-tests | 写测试 | P0 |
| tdd | TDD 开发 | P0 |
| finishing-a-development-branch | 分支管理 | P0 |
| code-review-excellence | 代码自审 | P1 |
| git-advanced-workflows | Git 高级操作 | P1 |
| refactor-code | 重构规范 | P1 |
| ultra-think | 复杂决策 | P1 |
| deps-audit | 依赖安全 | P1 |
| security-coding | 安全编码：JWT、CORS、密码哈希 | **P0** |
| frontend-best-practices | 前端规范：Error Boundary、Toast | **P1** |
| url-standards | URL 规范：不暴露内部路径 | **P1** |
| api-design-principles | API 设计 | P2 |
| error-analysis | 错误诊断 | P2 |
| code-explain | 代码解释 | P2 |
| tech-debt | 技术债务 | P2 |

## 同步到 Hermes

### 安装 Skill 文件到 Hermes Profile

```bash
# 进入仓库目录
cd /home/gql/repos/coder-ext-skill

# 执行同步脚本
bash sync-to-hermes.sh coder
```

同步后目录结构：
```
~/.hermes/profiles/coder/skills/
├── coder-ext-skill/                    # 路由器
│   ├── SKILL.md
│   └── references/
├── bmad-dev/                          # 独立 skill（软链接）
│   └── SKILL.md → coder-ext-skill/references/bmad-dev.md
├── debugging-strategies/               # 独立 skill（软链接）
│   └── SKILL.md → coder-ext-skill/references/debugging-strategies.md
└── ...                                 # 其他 skills
```

### 验证安装

```bash
# 查看已安装的 skills
ls -la ~/.hermes/profiles/coder/skills/

# 验证软链接
readlink -f ~/.hermes/profiles/coder/skills/bmad-dev/SKILL.md
```

---

## 工作流

详见 [SKILL.md](SKILL.md)

## 升级

详见 [update_readme.md](update_readme.md)
