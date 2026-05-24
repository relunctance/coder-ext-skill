# INSTALL.md - Coder Ext Skill 安装部署

## 前置要求

- Hermes Agent 已安装
- 目标 profile 已存在（如 `coder`）

## 安装步骤

### 方式 1：使用同步脚本（推荐）

```bash
# 进入仓库目录
cd /home/gql/repos/coder-ext-skill

# 执行同步脚本
python sync_to_hermes.py coder
```

### 方式 2：手动安装

```bash
# 1. 克隆仓库
git clone https://github.com/relunctance/coder-ext-skill.git ~/.hermes/profiles/coder/skills/coder-ext-skill

# 2. 进入目录
cd ~/.hermes/profiles/coder/skills/coder-ext-skill

# 3. 执行同步
python sync_to_hermes.py coder
```

## 验证安装

```bash
# 查看已安装的 skills
ls -la ~/.hermes/profiles/coder/skills/

# 验证软链接
readlink -f ~/.hermes/profiles/coder/skills/arc-ext-skill/SKILL.md
```

## 目录结构

安装后 `~/.hermes/profiles/coder/skills/` 应包含：

```
coder/
├── coder-ext-skill/            # 主 skill
│   ├── SKILL.md
│   ├── AGENTS.md
│   ├── INSTALL.md
│   ├── references/
│   │   ├── bmad-dev.md
│   │   ├── security-coding.md      # 新增
│   │   ├── frontend-best-practices.md  # 新增
│   │   └── url-standards.md       # 新增
│   └── shared/
├── bmad-dev/                   # 规范 skill
├── security-coding/
├── frontend-best-practices/
├── url-standards/
├── gql-coder/                  # 主角色 skill（来自 gql-bots）
└── ...
```

## 配置文件

### vars.md 配置

`coder/vars.md` 应包含：

```yaml
# Coder 配置变量
DOCS_HOME: {{GQL_BOTS_HOME}}/docs

## 通知模式
notify_mode: team  # report | team
HERMES_PROFILE: coder
FEISHU_MAIN: oc_22e019265c6096916f5a78de44f3cdea

## 模式配置
MODE_CONFIG: full_auto  # full_auto | semi_auto
```

## 更新 skill

```bash
cd /home/gql/repos/coder-ext-skill
git pull
python sync_to_hermes.py coder
```

## 卸载

```bash
rm -rf ~/.hermes/profiles/coder/skills/coder-ext-skill
rm -rf ~/.hermes/profiles/coder/skills/bmad-dev
rm -rf ~/.hermes/profiles/coder/skills/security-coding
rm -rf ~/.hermes/profiles/coder/skills/frontend-best-practices
rm -rf ~/.hermes/profiles/coder/skills/url-standards
```
