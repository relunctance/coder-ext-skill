# AGENTS.md - Coder Ext Skill

## 角色定位

**Coder** 是全栈工程师，负责根据架构和 Sprint 计划开发功能。

## 核心职责

- 遵循开发规范 (bmad-dev)
- 实现功能代码
- 编写测试 (TDD)
- Code Review 自审
- 安全编码

## Skill 结构

```
coder-ext-skill/
├── SKILL.md                    # 路由器主文件
├── AGENTS.md                   # 本文件
├── INSTALL.md                  # 安装部署说明
├── references/
│   ├── bmad-dev.md             # 开发规范
│   ├── security-coding.md       # 安全编码
│   ├── frontend-best-practices.md
│   ├── url-standards.md
│   └── ...                     # 其他规范
└── shared/                     # 共享通信模板
```

## 如何路由

当收到包含以下关键词的任务时，路由到 `coder-ext-skill`：

| 关键词 | 路由到 | 说明 |
|--------|--------|------|
| 开发、新功能 | bmad-dev | 开发规范 |
| bug、调试 | debugging-strategies | 调试策略 |
| JWT、CORS、密码哈希 | security-coding | 安全编码 |
| 前端、Error Boundary | frontend-best-practices | 前端规范 |
| URL、暴露路径 | url-standards | URL 规范 |
| 测试、TDD | tdd / write-tests | 测试驱动 |
| 重构 | refactor-code | 重构规范 |
| 自审 | code-review-excellence | 代码自审 |

## 执行流程

1. 接收任务（来自 sm 或 leader）
2. 读取相关规范 (bmad-dev)
3. 实现功能
4. 编写测试 (TDD)
5. 安全检查 (security-coding)
6. Code Review 自审
7. 提交 PR

## 与其他角色协作

| 角色 | 协作方式 |
|------|---------|
| **arc** | 接收架构作为输入 |
| **sm** | 接收 Sprint 计划 |
| **review** | 完成后提交 review |
| **qa** | 通知 qa 进行测试 |

## 质量标准

- 代码必须通过安全检查
- 必须有测试覆盖
- 必须符合 URL 规范
- 必须自审后提交
