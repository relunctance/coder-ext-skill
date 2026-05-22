# Coder Ext Skill 升级方案

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

# 复制所有 skill 文件
cp $PLUGINS/agent-team-agile-workflow/agents/bmad-dev.md $REFS/
cp $BASE/developer-essentials/skills/debugging-strategies/SKILL.md $REFS/debugging-strategies.md
cp $BASE/developer-essentials/skills/error-handling-patterns/SKILL.md $REFS/error-handling-patterns.md
cp $BASE/commands-code-analysis-testing/commands/write-tests.md $REFS/write-tests.md
cp $BASE/commands-code-analysis-testing/commands/tdd.md $REFS/tdd.md
cp $BASE/superpowers/skills/finishing-a-development-branch/SKILL.md $REFS/finishing-a-development-branch.md
cp $BASE/developer-essentials/skills/code-review-excellence/SKILL.md $REFS/code-review-excellence.md
cp $BASE/developer-essentials/skills/git-advanced-workflows/SKILL.md $REFS/git-advanced-workflows.md
cp $BASE/commands-utilities-debugging/commands/refactor-code.md $REFS/refactor-code.md
cp $BASE/commands-utilities-debugging/commands/ultra-think.md $REFS/ultra-think.md
cp $BASE/dependency-management/commands/deps-audit.md $REFS/deps-audit.md
cp $BASE/backend-development/skills/api-design-principles/SKILL.md $REFS/api-design-principles.md
cp $BASE/error-diagnostics/commands/error-analysis.md $REFS/error-analysis.md
cp $BASE/code-documentation/commands/code-explain.md $REFS/code-explain.md
cp $BASE/code-refactoring/commands/tech-debt.md $REFS/tech-debt.md
```

### Step 3: 重新添加 TL;DR（如有需要）

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
