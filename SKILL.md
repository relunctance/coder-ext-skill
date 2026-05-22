---
name: coder-ext-skill
description: Coder 技能索引路由器 - 接收任何开发任务，智能推荐最合适的 skill 并执行
version: 2.0.0
hermes:
  auto_route: true
---

# Coder Ext Skill - 智能技能路由器

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "开始开发" | bmad-dev | `hermes -p coder -s bmad-dev` |
| "调试 bug" | debugging-strategies | `hermes -p coder -s debugging-strategies` |
| "统一错误处理" | error-handling-patterns | `hermes -p coder -s error-handling-patterns` |
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

### 一句话触发规则

```
任务包含...         → 直接路由到...
────────────────────────────────────────────────────────────
"开发"、"新功能"、"开始" → bmad-dev
"bug"、"调试"、"报错" → debugging-strategies
"错误处理"、"异常" → error-handling-patterns
"测试"、"单元测试" → write-tests
"tdd"、"测试驱动" → tdd
"分支"、"合并"、"pr" → finishing-a-development-branch
"自审"、"code review" → code-review-excellence
"git"、"版本控制" → git-advanced-workflows
"重构"、"refactor" → refactor-code
"决策"、"方案评估" → ultra-think
"依赖"、"安全"、"漏洞" → deps-audit
"api"、"接口"、"rest" → api-design-principles
"分析错误"、"诊断" → error-analysis
"解释"、"说明代码" → code-explain
"技术债"、"debt" → tech-debt
```

## 🔀 智能路由决策树

```
收到 Coder 任务
    │
    ├─ 包含 "开发" / "新功能" / "开始"
    │   └─→ bmad-dev
    │
    ├─ 包含 "bug" / "调试" / "报错"
    │   └─→ debugging-strategies
    │       └─ error-analysis（如需深度诊断）
    │
    ├─ 包含 "错误处理" / "异常"
    │   └─→ error-handling-patterns
    │
    ├─ 包含 "测试" / "单元测试"
    │   └─→ write-tests
    │       └─ tdd（如需 TDD 方式）
    │
    ├─ 包含 "分支" / "合并" / "pr"
    │   └─→ finishing-a-development-branch
    │
    ├─ 包含 "自审" / "code review"
    │   └─→ code-review-excellence
    │
    ├─ 包含 "git" / "版本控制"
    │   └─→ git-advanced-workflows
    │
    ├─ 包含 "重构" / "refactor"
    │   └─→ refactor-code
    │
    ├─ 包含 "决策" / "方案评估"
    │   └─→ ultra-think
    │
    ├─ 包含 "依赖" / "安全" / "漏洞"
    │   └─→ deps-audit
    │
    ├─ 包含 "api" / "接口" / "rest"
    │   └─→ api-design-principles
    │
    ├─ 包含 "分析错误" / "诊断"
    │   └─→ error-analysis
    │
    ├─ 包含 "解释" / "说明代码"
    │   └─→ code-explain
    │
    ├─ 包含 "技术债" / "debt"
    │   └─→ tech-debt
    │
    └─ 无匹配
        └─→ bmad-dev（让开发核心判断）
```

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
| api-design-principles | API 设计：RESTful、GraphQL、版本管理 | P2 | api、接口、rest |
| error-analysis | 错误诊断：日志分析、堆栈跟踪 | P2 | 分析错误、诊断 |
| code-explain | 代码解释：注释生成、文档提取 | P2 | 解释、说明代码 |
| tech-debt | 技术债务：识别、跟踪、偿还策略 | P2 | 技术债、debt |

## 🎯 场景化深度参考

### 详细参考（引用）

**自然语言示例 + Fallback + 组合流** → 见 `references/quick-reference.md`

### 快速决策速查

```
新功能开发     → bmad-dev → write-tests/tdd → finishing-a-dev-branch
Bug 调试      → debugging-strategies → error-analysis
错误处理      → error-handling-patterns
代码自审      → code-review-excellence → deps-audit
重构          → refactor-code
复杂决策      → ultra-think
API 设计      → api-design-principles
技术债务      → tech-debt
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

## 📖 References 快速索引

详见 `references/quick-reference.md`（自然语言示例 + Fallback + 组合流）

每个 skill 文件都有 TL;DR 摘要：

| Skill | TL;DR | 行数 |
|-------|-------|------|
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
| api-design-principles | API 设计 | P2 |
| error-analysis | 错误诊断 | P2 |
| code-explain | 代码解释 | P2 |
| tech-debt | 技术债务 | P2 |

**注意**：`coder-ext-skill` 不会覆盖 `gql-coder` 主 skill，它们协同工作。
