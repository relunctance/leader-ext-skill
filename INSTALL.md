# INSTALL.md - Leader Ext Skill 安装部署

## 前置要求

- Hermes Agent 已安装
- 目标 profile 已存在（如 `leader`）

## 安装步骤

### 方式 1：使用同步脚本（推荐）

```bash
# 进入仓库目录
cd /home/gql/repos/leader-ext-skill

# 执行同步脚本
bash sync-to-hermes.sh leader
```

### 方式 2：手动安装

```bash
# 1. 克隆仓库
git clone https://github.com/relunctance/leader-ext-skill.git ~/.hermes/profiles/leader/skills/leader-ext-skill

# 2. 进入目录
cd ~/.hermes/profiles/leader/skills/leader-ext-skill

# 3. 执行同步
bash sync-to-hermes.sh leader
```

## 验证安装

```bash
# 查看已安装的 skills
ls -la ~/.hermes/profiles/leader/skills/

# 验证软链接
readlink -f ~/.hermes/profiles/leader/skills/leader-ext-skill/SKILL.md
```

## 目录结构

安装后 `~/.hermes/profiles/leader/skills/` 应包含：

```
leader/
├── leader-ext-skill/           # 主 skill
│   ├── SKILL.md
│   ├── AGENTS.md
│   ├── INSTALL.md
│   ├── references/
│   │   ├── context-manager/
│   │   ├── multi-agent-optimize/
│   │   └── create-buddy-agent/
│   └── shared/
├── gql-leader/                 # 主角色 skill（来自 gql-bots）
└── ...
```

## 配置文件

### vars.md 配置

`leader/vars.md` 应包含：

```yaml
# Leader 配置变量
FEISHU_MAIN: oc_22e019265c6096916f5a78de44f3cdea
GQL_BOTS_HOME: ~/.gql-bots
WORKSPACE_ENV: ~/.gql-bots/workspace.env
HERMES_PROFILES: ~/.hermes/profiles
WORKSPACE_ROOT: ~/tmp
MODE_CONFIG: full_auto
```

## 更新 skill

```bash
cd /home/gql/repos/leader-ext-skill
git pull
bash sync-to-hermes.sh leader
```

## 卸载

```bash
rm -rf ~/.hermes/profiles/leader/skills/leader-ext-skill
```
