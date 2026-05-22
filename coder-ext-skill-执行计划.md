# Coder Ext Skill 创建执行计划

## 目标

基于 `profile_role_skill.md` 和 `skills-catalog.md`，创建智能技能路由器仓库 `coder-ext-skill`，包含：
- ✅ 智能索引推荐功能（决策树 + 触发关键词 + 快速参考表）
- ✅ 所有 Coder skill 的 references（含 TL;DR 摘要）
- ✅ 升级方案
- ✅ learns 踩坑记录

---

## Step 1: 下载 & 解压插件包

```bash
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip
mkdir -p /home/gql/tmp/codebuddy-skills
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

---

## Step 2: 确认 GitHub 仓库

仓库名：`coder-ext-skill`

---

## Step 3: 复制 profile_role_skill.md

**Coder 角色 skill 列表**：

| Skill | 路径 | 级别 |
|-------|------|------|
| bmad-dev | `agent-team-agile-workflow/agents/bmad-dev.md` | P0 |
| debugging-strategies | `developer-essentials/skills/debugging-strategies/SKILL.md` | P0 |
| error-handling-patterns | `developer-essentials/skills/error-handling-patterns/SKILL.md` | P0 |
| write-tests | `commands-code-analysis-testing/commands/write-tests.md` | P0 |
| tdd | `commands-code-analysis-testing/commands/tdd.md` | P0 |
| finishing-a-development-branch | `superpowers/skills/finishing-a-development-branch/SKILL.md` | P0 |
| code-review-excellence | `developer-essentials/skills/code-review-excellence/SKILL.md` | P1 |
| git-advanced-workflows | `developer-essentials/skills/git-advanced-workflows/SKILL.md` | P1 |
| refactor-code | `commands-utilities-debugging/commands/refactor-code.md` | P1 |
| ultra-think | `commands-utilities-debugging/commands/ultra-think.md` | P1 |
| deps-audit | `dependency-management/commands/deps-audit.md` | P1 |
| api-design-principles | `backend-development/skills/api-design-principles/SKILL.md` | P2 |
| error-analysis | `error-diagnostics/commands/error-analysis.md` | P2 |
| code-explain | `code-documentation/commands/code-explain.md` | P2 |
| tech-debt | `code-refactoring/commands/tech-debt.md` | P2 |

---

## Step 4: 确认 skill 路径存在

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins

for skill in \
  "agent-team-agile-workflow/agents/bmad-dev.md" \
  "developer-essentials/skills/debugging-strategies/SKILL.md" \
  "developer-essentials/skills/error-handling-patterns/SKILL.md" \
  "developer-essentials/skills/code-review-excellence/SKILL.md" \
  "developer-essentials/skills/git-advanced-workflows/SKILL.md" \
  "commands-code-analysis-testing/commands/write-tests.md" \
  "commands-code-analysis-testing/commands/tdd.md" \
  "superpowers/skills/finishing-a-development-branch/SKILL.md" \
  "commands-utilities-debugging/commands/refactor-code.md" \
  "commands-utilities-debugging/commands/ultra-think.md" \
  "dependency-management/commands/deps-audit.md" \
  "backend-development/skills/api-design-principles/SKILL.md" \
  "error-diagnostics/commands/error-analysis.md" \
  "code-documentation/commands/code-explain.md" \
  "code-refactoring/commands/tech-debt.md"
do
  if [ -f "$PLUGINS/$skill" ] || [ -f "$BASE/$skill" ]; then
    echo "✅ $skill"
  else
    echo "❌ $skill NOT FOUND"
  fi
done
```

---

## Step 5: 阅读 coder_update.md

```bash
cat /home/gql/repos/gql-bots/docs/roles_skill/coder_update.md
```

---

## Step 6: 阅读 skills-catalog.md

**Coder 技能地图**：

| 场景 | 推荐 Skill | 理由 |
|------|-----------|------|
| 代码开发 | bmad-dev | 开发规范核心 |
| 调试问题 | debugging-strategies | 减少调试时间 |
| 错误处理 | error-handling-patterns | 统一错误处理 |
| 写测试 | write-tests | 保证代码质量 |
| TDD 开发 | tdd | 开发方法论 |
| 分支管理 | finishing-a-development-branch | 规范分支管理 |
| 自审代码 | code-review-excellence | 自审能力 |
| Git 高级操作 | git-advanced-workflows | Git 高级操作 |
| 代码重构 | refactor-code | 重构规范 |
| 复杂问题决策 | ultra-think | 复杂问题决策 |
| 依赖安全 | deps-audit | 依赖安全 |
| API 设计 | api-design-principles | API 设计规范 |
| 错误诊断 | error-analysis | 错误诊断 |
| 代码解释 | code-explain | 代码解释 |
| 技术债务管理 | tech-debt | 技术债务管理 |

---

## Step 7: 创建 coder-ext-skill 仓库

### 7.0 仓库初始化（重要！）

```bash
# 1. 创建 Gitee 仓库
curl -X POST "https://gitee.com/api/v5/user/repos" \
  -d "name=coder-ext-skill&description=Coder技能索引路由器&private=false&auto_init=false" \
  -H "Authorization: token 4995bfdbb1093963081f117438cc9b3a"

# 2. 创建本地仓库
mkdir -p /home/gql/repos/coder-ext-skill
cd /home/gql/repos/coder-ext-skill
git init
git remote add origin https://gitee.com/ztanfo_admin/coder-ext-skill.git
git remote add gitee https://ztanfo_admin:4995bfdbb1093963081f117438cc9b3a@gitee.com/ztanfo_admin/coder-ext-skill.git

# 3. 创建目录结构
mkdir -p /home/gql/repos/coder-ext-skill/learns
mkdir -p /home/gql/repos/coder-ext-skill/references
```

### 7.1 SKILL.md 设计要点

```markdown
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
| "检查依赖" | deps-audit | `hermes -p coder -s deps-audit` |
| "API 设计" | api-design-principles | `hermes -p coder -s api-design-principles` |
| "错误诊断" | error-analysis | `hermes -p coder -s error-analysis` |
| "解释代码" | code-explain | `hermes -p coder -s code-explain` |
| "技术债务" | tech-debt | `hermes -p coder -s tech-debt` |

### 一句话触发规则

```
任务包含...         → 直接路由到...
────────────────────────────────────────────────────────────
"开发"、"写代码"、"开始" → bmad-dev
"调试"、"debug"、"bug" → debugging-strategies
"错误"、"exception" → error-handling-patterns
"测试"、"test" → write-tests
"tdd"、"测试驱动" → tdd
"分支"、"merge"、"合并" → finishing-a-development-branch
"自审"、"review 自审" → code-review-excellence
"git"、"commit"、"branch" → git-advanced-workflows
"重构"、"refactor" → refactor-code
"复杂"、"决策" → ultra-think
"依赖"、"漏洞" → deps-audit
"api"、"接口" → api-design-principles
"诊断"、"分析错误" → error-analysis
"解释"、"说明代码" → code-explain
"债务"、"tech-debt" → tech-debt
```

## 🔀 智能路由决策树

```
收到开发任务
    │
    ├─ 包含 "开发" / "写代码" / "开始"
    │   └─→ bmad-dev
    │       ├─ 需要测试 → write-tests
    │       └─ TDD → tdd
    │
    ├─ 包含 "调试" / "debug" / "bug"
    │   └─→ debugging-strategies
    │
    ├─ 包含 "错误" / "exception"
    │   └─→ error-handling-patterns
    │
    ├─ 包含 "测试" / "test"
    │   └─→ write-tests
    │       └─ TDD → tdd
    │
    ├─ 包含 "分支" / "merge" / "合并"
    │   └─→ finishing-a-development-branch
    │
    ├─ 包含 "自审" / "review 自审"
    │   └─→ code-review-excellence
    │
    ├─ 包含 "git" / "commit" / "branch"
    │   └─→ git-advanced-workflows
    │
    ├─ 包含 "重构" / "refactor"
    │   └─→ refactor-code
    │
    ├─ 包含 "复杂" / "决策"
    │   └─→ ultra-think
    │
    ├─ 包含 "依赖" / "漏洞"
    │   └─→ deps-audit
    │
    ├─ 包含 "api" / "接口"
    │   └─→ api-design-principles
    │
    ├─ 包含 "诊断" / "分析错误"
    │   └─→ error-analysis
    │
    ├─ 包含 "解释" / "说明代码"
    │   └─→ code-explain
    │
    └─ 包含 "债务" / "tech-debt"
        └─→ tech-debt
```

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| bmad-dev | 开发规范核心：Git Flow、Code Style、Commit Convention | P0 | 开发、写代码、开始 |
| debugging-strategies | 调试策略：日志、 breakpoint、错误追踪 | P0 | 调试、debug、bug |
| error-handling-patterns | 错误处理：统一异常、错误码、日志规范 | P0 | 错误、exception |
| write-tests | 写测试：单元测试、集成测试、Mock | P0 | 测试、test |
| tdd | TDD开发：红绿重构、测试先行 | P0 | tdd、测试驱动 |
| finishing-a-development-branch | 分支管理：PR规范、Code Review、自审 | P0 | 分支、merge、合并 |
| code-review-excellence | 代码自审：自审清单、常见问题 | P1 | 自审、review自审 |
| git-advanced-workflows | Git高级：rebase、stash、cherry-pick | P1 | git、commit、branch |
| refactor-code | 重构规范：重构手法、安全重构 | P1 | 重构、refactor |
| ultra-think | 复杂决策：深度思考、方案评估 | P1 | 复杂、决策 |
| deps-audit | 依赖安全：漏洞扫描、版本管理 | P1 | 依赖、漏洞 |
| api-design-principles | API设计：RESTful、版本管理、文档 | P2 | api、接口 |
| error-analysis | 错误诊断：日志分析、堆栈追踪 | P2 | 诊断、分析错误 |
| code-explain | 代码解释：注释生成、逻辑说明 | P2 | 解释、说明代码 |
| tech-debt | 技术债务：识别、追踪、偿还策略 | P2 | 债务、tech-debt |

## 🎯 场景化深度参考

### 详细参考（引用）

**自然语言示例 + Fallback + 组合流** → 见 `references/quick-reference.md`

### 快速决策速查

```
开发任务        → bmad-dev
调试问题        → debugging-strategies
错误处理        → error-handling-patterns
写测试          → write-tests + tdd
分支管理        → finishing-a-development-branch
代码自审        → code-review-excellence
Git 高级       → git-advanced-workflows
重构            → refactor-code
复杂决策        → ultra-think
依赖安全        → deps-audit
API 设计        → api-design-principles
错误诊断        → error-analysis
代码解释        → code-explain
技术债务        → tech-debt
未知任务        → bmad-dev + 询问澄清
```

---

## 🗣️ 自然语言触发示例（引用）

**详细示例** → 见 `references/quick-reference.md`

---

## ❓ Fallback 处理

**当任务无法匹配任何规则时**：

```markdown
1. 询问用户澄清：
   "这个任务是开发、调试、测试、还是其他？"

2. 如果用户无法描述：
   → bmad-dev（让开发核心帮你判断）
```

---

## 🔗 任务组合流

```
新任务 → bmad-dev
       → write-tests + tdd（测试）
       → finishing-a-development-branch（分支）

Bug → debugging-strategies
    → error-analysis（诊断）
    → error-handling-patterns（修复）

提交前 → finishing-a-development-branch
       → code-review-excellence（自审）
       → deps-audit（依赖检查）
```

---

## 🔗 与 gql-coder 主 skill 联动

当 Coder 角色加载 `coder-ext-skill` 时：

```markdown
1. 收到开发任务
2. 先加载 coder-ext-skill（路由器）
3. 根据任务路由到具体 skill
4. 执行完成后返回 bmad-dev 做评审
```

**注意**：`coder-ext-skill` 不会覆盖 `gql-coder` 主 skill，它们协同工作。

---

## 🚨 常见错误

### 错误 1: 过度路由

```
❌ "用户说调试 bug，路由到 debugging-strategies，
    然后又问用户要不要用 error-analysis"
✓  直接路由到最可能的 skill，让用户决定是否深入
```

### 错误 2: 路由到不存在的 skill

```
❌ 根据关键词猜 skill 名称
✓  严格按照技能地图中的 skill 名称路由
```

### 错误 3: 忘记 Fallback

```
❌ 无法匹配时不知所措
✓  无法匹配时 → bmad-dev（让开发核心帮你判断）
```

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

查看 `update_readme.md` 了解如何同步最新 skill。

当前版本：v2.0.0

---

### 7.2 references 复制命令

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/coder-ext-skill/references

mkdir -p $REFS

# P0 Skills
cp $PLUGINS/agent-team-agile-workflow/agents/bmad-dev.md $REFS/bmad-dev.md
cp $BASE/developer-essentials/skills/debugging-strategies/SKILL.md $REFS/debugging-strategies.md
cp $BASE/developer-essentials/skills/error-handling-patterns/SKILL.md $REFS/error-handling-patterns.md
cp $BASE/commands-code-analysis-testing/commands/write-tests.md $REFS/write-tests.md
cp $BASE/commands-code-analysis-testing/commands/tdd.md $REFS/tdd.md
cp $BASE/superpowers/skills/finishing-a-development-branch/SKILL.md $REFS/finishing-a-development-branch.md

# P1 Skills
cp $BASE/developer-essentials/skills/code-review-excellence/SKILL.md $REFS/code-review-excellence.md
cp $BASE/developer-essentials/skills/git-advanced-workflows/SKILL.md $REFS/git-advanced-workflows.md
cp $BASE/commands-utilities-debugging/commands/refactor-code.md $REFS/refactor-code.md
cp $BASE/commands-utilities-debugging/commands/ultra-think.md $REFS/ultra-think.md
cp $BASE/dependency-management/commands/deps-audit.md $REFS/deps-audit.md

# P2 Skills
cp $BASE/backend-development/skills/api-design-principles/SKILL.md $REFS/api-design-principles.md
cp $BASE/error-diagnostics/commands/error-analysis.md $REFS/error-analysis.md
cp $BASE/code-documentation/commands/code-explain.md $REFS/code-explain.md
cp $BASE/code-refactoring/commands/tech-debt.md $REFS/tech-debt.md
```

### 7.3 references 添加 TL;DR

```bash
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    case "$f" in
      bmad-dev.md)
        echo "<!-- TL;DR: 开发规范核心：Git Flow、Code Style、Commit Convention -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      debugging-strategies.md)
        echo "<!-- TL;DR: 调试策略：日志、breakpoint、错误追踪 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      error-handling-patterns.md)
        echo "<!-- TL;DR: 错误处理：统一异常、错误码、日志规范 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      write-tests.md)
        echo "<!-- TL;DR: 写测试：单元测试、集成测试、Mock -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      tdd.md)
        echo "<!-- TL;DR: TDD开发：红绿重构、测试先行 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      finishing-a-development-branch.md)
        echo "<!-- TL;DR: 分支管理：PR规范、Code Review、自审 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      code-review-excellence.md)
        echo "<!-- TL;DR: 代码自审：自审清单、常见问题 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      git-advanced-workflows.md)
        echo "<!-- TL;DR: Git高级：rebase、stash、cherry-pick -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      refactor-code.md)
        echo "<!-- TL;DR: 重构规范：重构手法、安全重构 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      ultra-think.md)
        echo "<!-- TL;DR: 复杂决策：深度思考、方案评估 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      deps-audit.md)
        echo "<!-- TL;DR: 依赖安全：漏洞扫描、版本管理 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      api-design-principles.md)
        echo "<!-- TL;DR: API设计：RESTful、版本管理、文档 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      error-analysis.md)
        echo "<!-- TL;DR: 错误诊断：日志分析、堆栈追踪 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      code-explain.md)
        echo "<!-- TL;DR: 代码解释：注释生成、逻辑说明 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      tech-debt.md)
        echo "<!-- TL;DR: 技术债务：识别、追踪、偿还策略 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
    esac
  fi
done
```

---

## Step 8: 创建 learns/ 踩坑记录

**learns/README.md**：

```markdown
# Coder Ext Skill 踩坑沉淀

## 🏷️ 按标签索引

## #路径确认

### bmad-dev
- **位置**: `plugins/agent-team-agile-workflow/agents/bmad-dev.md`
- **注意**: bmad 系列在 plugins 目录

### developer-essentials/skills/*
- **位置**: `external_plugins/developer-essentials/skills/*/SKILL.md`
- **注意**: 多个 skill 在同一目录，需要遍历子目录

### commands-code-analysis-testing/commands/*
- **位置**: `external_plugins/commands-code-analysis-testing/commands/`
- **注意**: 这里是 .md 文件，不是 SKILL.md

### superpowers/skills/finishing-a-development-branch
- **位置**: `external_plugins/superpowers/skills/finishing-a-development-branch/SKILL.md`

## #source-区分

| 目录 | Skills |
|------|--------|
| `plugins/agent-team-agile-workflow/` | bmad-dev |
| `external_plugins/developer-essentials/skills/` | debugging-strategies, error-handling-patterns, code-review-excellence, git-advanced-workflows |
| `external_plugins/commands-code-analysis-testing/commands/` | write-tests, tdd |
| `external_plugins/superpowers/skills/` | finishing-a-development-branch |
| `external_plugins/commands-utilities-debugging/commands/` | refactor-code, ultra-think |
| `external_plugins/dependency-management/commands/` | deps-audit |
| `external_plugins/backend-development/skills/` | api-design-principles |
| `external_plugins/error-diagnostics/commands/` | error-analysis |
|| `external_plugins/code-documentation/commands/` | code-explain |
|| `external_plugins/code-refactoring/commands/` | tech-debt |
```

---

## Step 9: 创建 references/quick-reference.md

```bash
cat > /home/gql/repos/coder-ext-skill/references/quick-reference.md << 'EOF'
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
EOF
```

---

## Step 10: 创建 update_readme.md 升级方案

```bash
cat > /home/gql/repos/coder-ext-skill/update_readme.md << 'EOF'
# Coder Ext Skill 升级方案

## 执行计划

详见 `coder-ext-skill-执行计划.md`（详细步骤说明）

## 何时升级

1. `codebuddy-plugins-official.zip` 更新时
2. `gql-bots/shared/profile_role_skill.md` 变化时
3. `gql-bots/shared/skills-catalog.md` 更新时

## 升级步骤

### Step 1: 下载最新插件包

```bash
curl -L https://download.codebuddy.cn/plugin-marketplace/codebuddy-plugins-official.zip \
  -o /tmp/codebuddy-plugins-official.zip
unzip -o /tmp/codebuddy-plugins-official.zip -d /home/gql/tmp/codebuddy-skills
```

### Step 2: 同步 references

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins
REFS=/home/gql/repos/coder-ext-skill/references

# 重新复制所有 skill 文件
# [同 Step 7 的复制命令]
```

### Step 3: 重新添加 TL;DR

```bash
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    # 添加 TL;DR
    ...
  fi
done
```

### Step 4: 更新 quick-reference.md（如需要）

如果 skills-catalog.md 更新，同步更新 `references/quick-reference.md`。

### Step 5: 提交

```bash
cd /home/gql/repos/coder-ext-skill
git add -A
git commit -m "chore: sync with latest codebuddy-plugins"
git push origin main
```

## 版本号规则

| 类型 | 规则 |
|------|------|
| 主版本 | skill 索引结构变化、决策树重构 |
| 次版本 | 新增/删除 skill、触发关键词更新 |
| 修订版 | 内容更新、TL;DR 更新 |

## 路径速查

| Skill | 源路径 |
|-------|--------|
| bmad-dev | `plugins/agent-team-agile-workflow/agents/bmad-dev.md` |
| debugging-strategies | `external_plugins/developer-essentials/skills/debugging-strategies/SKILL.md` |
| error-handling-patterns | `external_plugins/developer-essentials/skills/error-handling-patterns/SKILL.md` |
| write-tests | `external_plugins/commands-code-analysis-testing/commands/write-tests.md` |
| tdd | `external_plugins/commands-code-analysis-testing/commands/tdd.md` |
| finishing-a-development-branch | `external_plugins/superpowers/skills/finishing-a-development-branch/SKILL.md` |
| code-review-excellence | `external_plugins/developer-essentials/skills/code-review-excellence/SKILL.md` |
| git-advanced-workflows | `external_plugins/developer-essentials/skills/git-advanced-workflows/SKILL.md` |
| refactor-code | `external_plugins/commands-utilities-debugging/commands/refactor-code.md` |
| ultra-think | `external_plugins/commands-utilities-debugging/commands/ultra-think.md` |
| deps-audit | `external_plugins/dependency-management/commands/deps-audit.md` |
| api-design-principles | `external_plugins/backend-development/skills/api-design-principles/SKILL.md` |
| error-analysis | `external_plugins/error-diagnostics/commands/error-analysis.md` |
| code-explain | `external_plugins/code-documentation/commands/code-explain.md` |
| tech-debt | `external_plugins/code-refactoring/commands/tech-debt.md` |
EOF
```

---

## Step 11: 创建 README.md

```bash
cat > /home/gql/repos/coder-ext-skill/README.md << 'EOF'
# Coder Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/Version-1.0.0-green.svg)](SKILL.md)
[![Coder Skills](https://img.shields.io/badge/Coder_Skills-15-orange.svg)](#)
[![Auto Route](https://img.shields.io/badge/Auto_Route-Enabled-blue.svg)](#)

开发者技能索引路由器 - 接收任何开发任务，智能推荐最合适的 skill 并执行。

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
| api-design-principles | API 设计 | P2 |
| error-analysis | 错误诊断 | P2 |
| code-explain | 代码解释 | P2 |
| tech-debt | 技术债务 | P2 |

## 工作流

详见 [SKILL.md](SKILL.md)

## 升级

详见 [update_readme.md](update_readme.md)
EOF
```

---

## Step 12: Git 提交

```bash
cd /home/gql/repos/coder-ext-skill
git add -A
git commit -m "feat: initial coder-ext-skill with all Coder skills"
git push origin main
```

---

## 验证清单

- [ ] 下载解压成功
- [ ] GitHub 仓库已创建/更新
- [ ] 所有 15 个 skill 路径验证通过
- [ ] references/ 包含所有 skill 文件（含 TL;DR）
- [ ] references/quick-reference.md 已创建
- [ ] SKILL.md 包含智能索引：
  - [ ] 一句话触发规则
  - [ ] 决策树
  - [ ] 技能地图
  - [ ] 场景化深度参考
  - [ ] Fallback 处理
  - [ ] 任务组合流
  - [ ] 主 skill 联动
- [ ] learns/ 有踩坑记录
- [ ] update_readme.md 有升级方案
- [ ] README.md 已美化
- [ ] git push 成功
