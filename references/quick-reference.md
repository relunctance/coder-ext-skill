<!-- TL;DR: 自然语言示例 + Fallback + 组合流快速参考 -->

# Coder 技能路由快速参考

> 详细决策树见 SKILL.md 主文件

## 🗣️ 自然语言触发示例

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "开始新功能开发" | bmad-dev | 开发规范 |
| "遇到 bug 了" | debugging-strategies | 调试 |
| "帮我看看这个错误" | error-handling-patterns | 错误处理 |
| "写个单元测试" | write-tests | 写测试 |
| "用 TDD 方式" | tdd | TDD 开发 |
| "分支合并要注意什么" | finishing-a-development-branch | 分支管理 |
| "提交前自审" | code-review-excellence | 代码自审 |
| "rebase 怎么用" | git-advanced-workflows | Git 高级 |
| "这段代码要重构" | refactor-code | 重构 |
| "这个决策怎么做" | ultra-think | 复杂决策 |
| "检查下依赖安全" | deps-audit | 依赖检查 |
| "设计一个 API" | api-design-principles | API 设计 |
| "分析下这个报错" | error-analysis | 错误诊断 |
| "解释下这段代码" | code-explain | 代码解释 |
| "技术债太多了" | tech-debt | 技术债务 |

---

## 🔄 Fallback 处理

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

**Fallback 规则**：
```markdown
无匹配时 → bmad-dev → 让他判断用哪个 skill
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

## ⚡ 快速决策速查卡

```
┌─────────────────────────────────────────────────────────────┐
│  任务类型              │  首选 Skill         │  辅助      │
├─────────────────────────────────────────────────────────────┤
│  新功能开发            │  bmad-dev          │            │
│  Bug 调试              │  debugging-strat.  │  error-ana.│
│  错误处理              │  error-handling-p. │            │
│  写测试                │  write-tests       │  tdd       │
│  TDD 开发              │  tdd              │            │
│  分支管理              │  finishing-a-dev-b.│            │
│  代码自审              │  code-review-exc.  │            │
│  Git 高级              │  git-advanced-w.  │            │
│  重构                  │  refactor-code     │            │
│  复杂决策              │  ultra-think       │            │
│  依赖安全              │  deps-audit        │            │
│  API 设计              │  api-design-prin.  │            │
│  错误诊断              │  error-analysis    │            │
│  代码解释              │  code-explain      │            │
│  技术债务              │  tech-debt         │            │
│  未知任务              │  bmad-dev          │  询问澄清  │
└─────────────────────────────────────────────────────────────┘
```
