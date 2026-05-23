---
name: coder-ext-skill
description: Coder 技能索引路由器 - 接收任何开发任务，智能推荐最合适的 skill 并执行
version: 2.1.0
author: relunctance
license: MIT
category: gql-bots
tags:
  - coder
  - development
  - skill-router
  - gql-bots
  - intelligent-router
hermes:
  platforms:
    hermes: true
  auto_route: true
---

# Coder Ext Skill - 智能技能路由器

> **核心定位**：Coder 角色的中央路由器。任何开发任务进来，先查这里，再路由到具体 skill。

---

## ⚡ TL;DR 速查索引

| 你要做的事 | 直接路由 | 说明 |
|------------|---------|------|
| 开始开发 | bmad-dev | 开发规范 |
| 调试 bug | debugging-strategies | 调试策略 |
| 写测试 | write-tests / tdd | 单元/集成测试 |
| 合并分支/PR | finishing-a-development-branch | PR 规范 |
| 代码自审 | code-review-excellence | 自审清单 |
| 重构代码 | refactor-code | 安全重构 |
| 依赖安全 | deps-audit | 漏洞扫描 |
| 复杂决策 | ultra-think | 深度思考 |
| 解释代码 | code-explain | 注释生成 |
| **安全编码** | security-coding | JWT、CORS、密码哈希 |
| **前端规范** | frontend-best-practices | Error Boundary、Toast |
| **URL 规范** | url-standards | 不暴露内部路径 |
| 不知道用哪个 | bmad-dev | 让它帮你判断 |

---

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "开始开发" | bmad-dev | `hermes -p coder -s bmad-dev` |
| "调试 bug" | debugging-strategies | `hermes -p coder -s debugging-strategies` |
| "安全编码"、"JWT"、"密码哈希"、"CORS" | security-coding | `hermes -p coder -s security-coding` |
| "前端规范"、"Error Boundary"、"Toast" | frontend-best-practices | `hermes -p coder -s frontend-best-practices` |
| "URL"、"暴露路径"、"访问地址" | url-standards | `hermes -p coder -s url-standards` |
| "写测试" | write-tests | `hermes -p coder -s write-tests` |
| "TDD 开发" | tdd | `hermes -p coder -s tdd` |
| "合并分支" | finishing-a-development-branch | `hermes -p coder -s finishing-a-development-branch` |
| "代码自审" | code-review-excellence | `hermes -p coder -s code-review-excellence` |
| "Git 操作" | git-advanced-workflows | `hermes -p coder -s git-advanced-workflows` |
| "重构代码" | refactor-code | `hermes -p coder -s refactor-code` |
| "复杂决策" | ultra-think | `hermes -p coder -s ultra-think` |
| "依赖安全" | deps-audit | `hermes -p coder -s deps-audit` |
| "设计 API" | api-design-principles | `hermes -p coder -s api-design-principles` |
| "分析错误" | error-analysis | `hermes -p coder -s error-analysis` |
| "解释代码" | code-explain | `hermes -p coder -s code-explain` |
| "技术债务" | tech-debt | `hermes -p coder -s tech-debt` |

### 一句话触发规则（增强版）

```
任务包含...              → 直接路由到...
────────────────────────────────────────────────────────────────────────────
# 开发流程
"开发"、"新功能"、"开始" → bmad-dev
"任务分解"、"分解任务" → bmad-dev
"代码规范"、"风格" → bmad-dev

# 调试/错误
"bug"、"调试"、"报错" → debugging-strategies
"错误处理"、"异常" → error-handling-patterns
"分析错误"、"诊断" → error-analysis
"堆栈"、"stack"、"日志" → error-analysis

# 测试
"测试"、"单元测试"、"写测试" → write-tests
"tdd"、"测试驱动"、"红绿" → tdd
"集成测试"、"e2e" → write-tests
"测试覆盖"、"coverage" → write-tests

# 分支/Git
"分支"、"合并"、"pr" → finishing-a-development-branch
"git"、"版本控制"、"commit" → git-advanced-workflows
"rebase"、"cherry-pick" → git-advanced-workflows
"自审"、"code review" → code-review-excellence

# 重构/优化
"重构"、"refactor"、"优化" → refactor-code
"性能优化"、"代码优化" → refactor-code

# 决策/分析
"决策"、"方案评估"、"选型" → ultra-think
"分析"、"评估"、"比较" → ultra-think

# 依赖/安全
"依赖"、"漏洞"、"安全" → deps-audit
"npm audit"、"安全扫描" → deps-audit

# 安全编码（新增）
"JWT"、"secret"、"密码哈希"、"CORS"、"token黑名单" → security-coding

# 前端规范（新增）
"前端"、"Error Boundary"、"Toast"、"错误处理" → frontend-best-practices

# URL 规范（新增）
"URL"、"暴露路径"、"访问地址"、"交付" → url-standards

# API 设计
"api"、"接口"、"rest" → api-design-principles
"endpoint"、"路由" → api-design-principles

# 代码解释
"解释"、"说明"、"注释" → code-explain
"文档"、"注释生成" → code-explain

# 技术债
"技术债"、"debt"、"债" → tech-debt
```

---

## 🔀 智能路由决策树

```
收到 Coder 任务
    │
    ├─ 🎯 开发流程？
    │   └─ bmad-dev
    │         ├─ 任务分解
    │         ├─ 代码规范
    │         └─ Git flow
    │
    ├─ 🎯 调试/错误？
    │   ├─ debugging-strategies（调试策略）
    │   ├─ error-handling-patterns（错误处理）
    │   └─ error-analysis（深度诊断）
    │
    ├─ 🎯 测试？
    │   ├─ write-tests（写测试）
    │   └─ tdd（TDD 开发）
    │
    ├─ 🎯 分支/Git？
    │   ├─ finishing-a-development-branch（合并/PR）
    │   └─ git-advanced-workflows（高级 Git）
    │
    ├─ 🎯 重构/优化？
    │   └─ refactor-code
    │
    ├─ 🎯 决策/分析？
    │   └─ ultra-think
    │
    ├─ 🎯 依赖/安全？
    │   └─ deps-audit
    │
    ├─ 🎯 API 设计？
    │   └─ api-design-principles
    │
    ├─ 🎯 代码解释？
    │   └─ code-explain
    │
    └─ ❓ 不知道
        └─ bmad-dev + 询问澄清
```

---

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| bmad-dev | 开发规范核心：任务分解、代码规范、Git flow | P0 | 开发、新功能、开始 |
| debugging-strategies | 调试策略：日志、断点、问题定位 | P0 | bug、调试、报错 |
| error-handling-patterns | 错误处理：异常设计、边界处理 | P0 | 错误处理、异常 |
| write-tests | 写测试：单元测试、集成测试、E2E | P0 | 测试、单元测试 |
| tdd | TDD 开发：红绿重构、测试先行 | P0 | tdd、测试驱动 |
| finishing-a-development-branch | 分支管理：PR规范、Code Review、自审 | P0 | 分支、合并、pr |
| code-review-excellence | 代码自审：自审清单、常见问题 | P1 | 自审、code review |
| git-advanced-workflows | Git 高级：rebase、stash、cherry-pick | P1 | git、版本控制 |
| refactor-code | 重构规范：重构手法、安全重构 | P1 | 重构、refactor |
| ultra-think | 复杂决策：深度思考、方案评估 | P1 | 决策、方案评估 |
| deps-audit | 依赖安全：漏洞扫描、版本管理 | P1 | 依赖、安全、漏洞 |
| **security-coding** | **安全编码：JWT、CORS、密码哈希、Token** | **P0** | **JWT、CORS、密码哈希** |
| **frontend-best-practices** | **前端规范：Error Boundary、Toast** | **P1** | **前端、Error Boundary** |
| **url-standards** | **URL 规范：不暴露内部路径** | **P1** | **URL、暴露路径、交付** |
| api-design-principles | API 设计：RESTful、GraphQL、版本管理 | P2 | api、接口、rest |
| error-analysis | 错误诊断：日志分析、堆栈跟踪 | P2 | 分析错误、诊断 |
| code-explain | 代码解释：注释生成、文档提取 | P2 | 解释、说明代码 |
| tech-debt | 技术债务：识别、跟踪、偿还策略 | P2 | 技术债、debt |

---

## 🎯 场景化深度参考（4大场景）

### 场景 1: 新功能开发 🆕

```
需求：开始一个新功能
    │
    └─ bmad-dev（遵循规范）
          ├─ 任务分解
          │     → 子任务拆分
          │     → 依赖关系
          │
          ├─ 开发
          │     → bmad-dev
          │     → write-tests（测试）
          │     → tdd（如需 TDD）
          │
          └─ 收尾
                → finishing-a-development-branch
                → code-review-excellence（自审）
                → deps-audit（依赖检查）
```

### 场景 2: Bug 调试 🔍

```
需求：线上报错了
    │
    ├─ debugging-strategies（调试策略）
    │     → 日志分析
    │     → 断点设置
    │     → 问题定位
    │
    └─ error-analysis（如需深度诊断）
          → 堆栈跟踪
          → 根因分析
```

### 场景 3: 代码重构 🛠️

```
需求：需要重构代码
    │
    └─ refactor-code
          ├─ 重构前
          │     → 识别坏味道
          │     → 编写测试保护网
          │
          ├─ 重构中
          │     → 小步重构
          │     → 频繁测试
          │
          └─ 重构后
                → 测试通过
                → code-review-excellence
```

### 场景 4: 分支管理 🔀

```
需求：准备合并分支
    │
    └─ finishing-a-development-branch
          ├─ 自审
          │     → code-review-excellence
          │     → 自审清单
          │
          ├─ 依赖检查
          │     → deps-audit
          │
          └─ Git 操作
                → git rebase（如需）
                → 创建 PR
                → code review
```

### 快速决策速查

```
┌────────────────────────────────────────────────────────────┐
│  场景              │  路由顺序                              │
├────────────────────────────────────────────────────────────┤
│  新功能开发         │  bmad-dev → write-tests → finishing  │
│  Bug 调试          │  debugging-strategies → error-analysis │
│  错误处理          │  error-handling-patterns              │
│  代码自审          │  code-review-excellence → deps-audit  │
│  重构              │  refactor-code                       │
│  复杂决策          │  ultra-think                         │
│  API 设计          │  api-design-principles                │
│  依赖安全          │  deps-audit                          │
│  代码解释          │  code-explain                        │
│  技术债务          │  tech-debt                           │
│  未知任务          │  bmad-dev + 询问澄清                 │
└────────────────────────────────────────────────────────────┘
```

---

## ❓ Fallback 处理

当任务**无法匹配**以上任何规则时：

```
未知任务
    │
    ├─ 询问用户澄清：
    │   "这个任务是开发、调试、测试、还是其他？"
    │
    └─ 如果用户无法描述：
        └─→ bmad-dev（让开发核心帮你判断）
```

---

## 🔗 任务组合流

### 组合 1: 新功能开发

```
"开始新功能"
    │
    ├─ bmad-dev（遵循规范）
    │     ├─ write-tests + tdd（测试）
    │     ├─ api-design-principles（如需 API）
    │     └─ refactor-code（如需重构）
    │
    └─ finishing-a-development-branch（收尾）
          ├─ code-review-excellence（自审）
          └─ deps-audit（依赖检查）
```

### 组合 2: Bug 调试

```
"遇到 bug"
    │
    ├─ debugging-strategies（调试策略）
    │     └─ error-analysis（深度诊断）
    │
    └─ error-handling-patterns（修复方案）
```

### 组合 3: 代码提交

```
"准备合并分支"
    │
    ├─ finishing-a-development-branch
    │     ├─ code-review-excellence（自审）
    │     └─ deps-audit（依赖检查）
    │
    └─ git-advanced-workflows（如需高级 Git）
```

---

## 🔗 与 gql-coder 主 skill 联动

**注意**：`coder-ext-skill` 不会覆盖 `gql-coder` 主 skill，它们协同工作。

```
┌─────────────────────────────────────────────────────────────┐
│  gql-coder 主 skill                                        │
│    │                                                        │
│    ├─ 通用开发任务 → coder-ext-skill（路由）                │
│    │              └─→ 具体 skill 执行                         │
│    │                                                        │
│    └─ 特定技能任务 → 直接调用具体 skill                       │
└─────────────────────────────────────────────────────────────┘
```

**何时使用 coder-ext-skill**：
- 任务模糊，需要判断用哪个 skill
- 复杂任务需要多 skill 组合
- 不确定某个 skill 是否适用

**何时直接调用具体 skill**：
- 任务明确，比如"用 TDD 开发"
- 已确定需要哪个 skill
- 只需要单个 skill

---

## 📖 References 快速索引

详见 `references/quick-reference.md`（自然语言示例 + Fallback + 组合流）

每个 skill 文件都有 TL;DR 摘要：

| Skill | TL;DR | 说明 |
|-------|-------|------|
| bmad-dev.md | 开发规范核心 | 任务分解、代码规范 |
| debugging-strategies.md | 调试策略 | 日志、断点、定位 |
| error-handling-patterns.md | 错误处理 | 异常、边界 |
| write-tests.md | 写测试 | 单元、集成、E2E |
| tdd.md | TDD 开发 | 红绿重构 |
| finishing-a-development-branch.md | 分支管理 | PR、自审 |
| code-review-excellence.md | 代码自审 | 清单、问题 |
| git-advanced-workflows.md | Git 高级 | rebase、cherry-pick |
| refactor-code.md | 重构规范 | 安全重构 |
| ultra-think.md | 复杂决策 | 深度思考 |
| deps-audit.md | 依赖安全 | 漏洞扫描 |
| **security-coding.md** | **安全编码** | **JWT、CORS、密码哈希** |
| **frontend-best-practices.md** | **前端规范** | **Error Boundary、Toast** |
| **url-standards.md** | **URL 规范** | **不暴露内部路径** |
| api-design-principles.md | API 设计 | RESTful、GraphQL |
| error-analysis.md | 错误诊断 | 日志、堆栈 |
| code-explain.md | 代码解释 | 注释、文档 |
| tech-debt.md | 技术债务 | 识别、偿还 |

---

## 🚨 常见错误

| 错误 | 正确做法 |
|------|---------|
| 直接说"开发" | 说明开发什么（新功能/bug修复/重构） |
| 不确定用哪个 skill | → bmad-dev 让它帮你判断 |
| 过度路由 | 直接路由到最可能的 skill |
| 忘记 Fallback | 无法匹配时 → bmad-dev |

---

## 🔗 相关角色联动

| 角色 | 协作场景 |
|------|---------|
| **arc** | 架构决策时：先 arc-ext-skill 评审架构，再 coder-ext-skill 执行 |
| **review** | 代码完成后：coder-ext-skill 自审后 → review-ext-skill 正式评审 |
| **sm** | 任务分解时：sm-ext-skill 规划任务 → coder-ext-skill 执行 |
| **qa** | 测试相关：coder-ext-skill 写测试 → qa-ext-skill 验证 |

---

## 🗣️ 示例对话

### 示例 1: 路由到 bmad-dev

```
用户：开始新功能开发
AI：分析：包含"开发"、"新功能"
     路由到 bmad-dev
     执行：开发规范核心流程
```

### 示例 2: 路由到调试组合

```
用户：线上报错了
AI：分析：包含"报错"
     路由到 debugging-strategies
     执行：调试策略
     建议：是否需要 error-analysis 深度诊断？
```

### 示例 3: Fallback 处理

```
用户：帮我看看这个
AI：分析：无法匹配关键词
     Fallback → bmad-dev
     执行：开发核心判断任务类型
     反馈：这个任务是"调试 bug"，建议用 debugging-strategies
```

---

## 升级说明

查看 [update_readme.md](update_readme.md) 了解如何同步最新 skill。

当前版本：v2.0.0
