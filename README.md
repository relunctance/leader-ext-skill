# Leader Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/Version-2.0.0-green.svg)](SKILL.md)
[![Leader Skills](https://img.shields.io/badge/Leader_Skills-6-orange.svg)](#)
[![Auto Route](https://img.shields.io/badge/Auto_Route-Enabled-blue.svg)](#)

团队协调技能索引路由器 - 接收任何管理任务，智能推荐最合适的 skill 并执行。

## 一句话路由规则

```
收到管理任务
    │
    ├─ "上下文"/"状态" → context-manager
    ├─ "多 Agent"/"协作" → multi-agent-optimize
    ├─ "创建 Agent" → create-buddy-agent
    └─ 无匹配 → context-manager
```

## 目录

- [快速开始](#快速开始)
- [技能地图](#技能地图)
- [工作流](#工作流)
- [升级](#升级)

## 快速开始

```bash
# 安装
git clone https://gitee.com/ztanfo_admin/leader-ext-skill.git ~/.hermes/profiles/leader/skills/leader-ext-skill

# 使用
hermes -p leader -s leader-ext-skill
```

## 技能地图

| Skill | 说明 | 级别 |
|-------|------|------|
| context-manager | 上下文管理 | P0 |
| multi-agent-optimize | 多 Agent 协作 | P0 |
| create-buddy-agent | Agent 创建 | P0 |
| project-architect | 项目架构 | P1 |

## 同步到 Hermes

### 安装 Skill 文件到 Hermes Profile

```bash
# 进入仓库目录
cd /home/gql/repos/leader-ext-skill

# 执行同步脚本
bash sync-to-hermes.sh leader
```

同步后目录结构：
```
~/.hermes/profiles/leader/skills/
├── leader-ext-skill/                    # 路由器
│   ├── SKILL.md
│   └── references/
├── context-manager/                  # 独立 skill（软链接）
│   └── SKILL.md → leader-ext-skill/references/context-manager.md
├── multi-agent-optimize/             # 独立 skill（软链接）
│   └── SKILL.md → leader-ext-skill/references/multi-agent-optimize.md
└── ...                               # 其他 skills
```

### 验证安装

```bash
# 查看已安装的 skills
ls -la ~/.hermes/profiles/leader/skills/

# 验证软链接
readlink -f ~/.hermes/profiles/leader/skills/context-manager/SKILL.md
```

---

## 工作流

详见 [SKILL.md](SKILL.md)

## 升级

详见 [update_readme.md](update_readme.md)
