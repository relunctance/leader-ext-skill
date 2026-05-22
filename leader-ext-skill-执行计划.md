# Leader Ext Skill 创建执行计划

## 目标

基于 `profile_role_skill.md` 和 `skills-catalog.md`，创建智能技能路由器仓库 `leader-ext-skill`，包含：
- ✅ 智能索引推荐功能（决策树 + 触发关键词 + 快速参考表）
- ✅ 所有 Leader skill 的 references（含 TL;DR 摘要）
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

仓库名：`leader-ext-skill`

---

## Step 3: 复制 profile_role_skill.md

**Leader 角色 skill 列表**：

| Skill | 路径 | 级别 |
|-------|------|------|
| context-manager | `agent-orchestration/agents/context-manager.md` | P0 |
| project-health-check | `commands-project-task-management/commands/project-health-check.md` | P1 |
| milestone-tracker | `commands-project-task-management/commands/milestone-tracker.md` | P1 |
| multi-agent-optimize | `agent-orchestration/commands/multi-agent-optimize.md` | P1 |
| create-prd | `commands-project-task-management/commands/create-prd.md` | P1 |
| project-timeline-simulator | `commands-project-task-management/commands/project-timeline-simulator.md` | P2 |
| deployment-engineer | `cicd-automation/agents/deployment-engineer.md` | P2 |
| performance-engineer | `observability-monitoring/agents/performance-engineer.md` | P2 |

**注意**：`multi-agent-optimize` 实际路径是 `agent-orchestration/commands/multi-agent-optimize.md`（commands 目录，不是 agents）

---

## Step 4: 确认 skill 路径存在

```bash
BASE=/home/gql/tmp/codebuddy-skills/external_plugins
PLUGINS=/home/gql/tmp/codebuddy-skills/plugins

for skill in \
  "agent-orchestration/agents/context-manager.md" \
  "agent-orchestration/commands/multi-agent-optimize.md" \
  "commands-project-task-management/commands/project-health-check.md" \
  "commands-project-task-management/commands/milestone-tracker.md" \
  "commands-project-task-management/commands/create-prd.md" \
  "commands-project-task-management/commands/project-timeline-simulator.md" \
  "cicd-automation/agents/deployment-engineer.md" \
  "observability-monitoring/agents/performance-engineer.md"
do
  if [ -f "$PLUGINS/$skill" ] || [ -f "$BASE/$skill" ]; then
    echo "✅ $skill"
  else
    echo "❌ $skill NOT FOUND"
  fi
done
```

---

## Step 5: 阅读 leader_update.md

```bash
cat /home/gql/repos/gql-bots/docs/roles_skill/leader_update.md
```

---

## Step 6: 阅读 skills-catalog.md

**Leader 技能地图**：

| 场景 | 推荐 Skill | 理由 |
|------|-----------|------|
| 多 Agent 任务协调 | context-manager | 多 Agent 上下文协调 |
| 项目健康检查 | project-health-check | 项目健康监控 |
| 里程碑追踪 | milestone-tracker | 里程碑追踪 |
| 多 Agent 性能优化 | multi-agent-optimize | 多 Agent 性能优化 |
| PRD 模板化 | create-prd | PRD 模板化 |
| 预测分析 | project-timeline-simulator | 预测分析 |
| 部署编排 | deployment-engineer | 部署编排 |
| 性能优化 | performance-engineer | 性能优化 |

---

## Step 7: 创建 leader-ext-skill 仓库

### 7.0 仓库初始化（重要！）

```bash
# 1. 创建 Gitee 仓库
curl -X POST "https://gitee.com/api/v5/user/repos" \
  -d "name=leader-ext-skill&description=Leader技能索引路由器&private=false&auto_init=false" \
  -H "Authorization: token 4995bfdbb1093963081f117438cc9b3a"

# 2. 创建本地仓库
mkdir -p /home/gql/repos/leader-ext-skill
cd /home/gql/repos/leader-ext-skill
git init
git remote add origin https://gitee.com/ztanfo_admin/leader-ext-skill.git
git remote add gitee https://ztanfo_admin:4995bfdbb1093963081f117438cc9b3a@gitee.com/ztanfo_admin/leader-ext-skill.git

# 3. 创建目录结构
mkdir -p /home/gql/repos/leader-ext-skill/learns
mkdir -p /home/gql/repos/leader-ext-skill/references
```

### 7.1 SKILL.md 设计要点

```markdown
---
name: leader-ext-skill
description: Leader 技能索引路由器 - 接收任何领导/协调任务，智能推荐最合适的 skill 并执行
version: 2.0.0
hermes:
  auto_route: true
---

# Leader Ext Skill - 智能技能路由器

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "多 Agent 协调" | context-manager | `hermes -p leader -s context-manager` |
| "健康检查" | project-health-check | `hermes -p leader -s project-health-check` |
| "里程碑" | milestone-tracker | `hermes -p leader -s milestone-tracker` |
| "Agent 优化" | multi-agent-optimize | `hermes -p leader -s multi-agent-optimize` |
| "写 PRD" | create-prd | `hermes -p leader -s create-prd` |
| "预测进度" | project-timeline-simulator | `hermes -p leader -s project-timeline-simulator` |
| "部署" | deployment-engineer | `hermes -p leader -s deployment-engineer` |
| "性能优化" | performance-engineer | `hermes -p leader -s performance-engineer` |

### 一句话触发规则

```
任务包含...         → 直接路由到...
────────────────────────────────────────────────────────────
"多 agent"、"协调"、"上下文" → context-manager
"健康"、"health"、"检查" → project-health-check
"里程碑"、"milestone" → milestone-tracker
"agent 优化"、"性能" → multi-agent-optimize
"prd"、"需求文档" → create-prd
"预测"、"timeline"、"进度" → project-timeline-simulator
"部署"、"deployment" → deployment-engineer
"性能"、"performance" → performance-engineer
```

## 🔀 智能路由决策树

```
收到领导任务
    │
    ├─ 包含 "多 agent" / "协调" / "上下文"
    │   └─→ context-manager
    │
    ├─ 包含 "健康" / "health" / "检查"
    │   └─→ project-health-check
    │
    ├─ 包含 "里程碑" / "milestone"
    │   └─→ milestone-tracker
    │
    ├─ 包含 "agent 优化" / "性能"
    │   └─→ multi-agent-optimize
    │
    ├─ 包含 "prd" / "需求文档"
    │   └─→ create-prd
    │
    ├─ 包含 "预测" / "timeline" / "进度"
    │   └─→ project-timeline-simulator
    │
    ├─ 包含 "部署" / "deployment"
    │   └─→ deployment-engineer
    │
    └─ 包含 "性能" / "performance"
        └─→ performance-engineer
```

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| context-manager | 多Agent上下文协调：跨Agent状态共享、冲突检测 | P0 | 多agent、协调、上下文 |
| project-health-check | 项目健康监控：进度、风险、阻塞 | P1 | 健康、health、检查 |
| milestone-tracker | 里程碑追踪：进度监控、报告 | P1 | 里程碑、milestone |
| multi-agent-optimize | 多Agent性能优化：调度、负载均衡 | P1 | agent优化、性能 |
| create-prd | PRD模板化：PRD文档标准化 | P1 | prd、需求文档 |
| project-timeline-simulator | 预测分析：Burndown、进度预测 | P2 | 预测、timeline、进度 |
| deployment-engineer | 部署编排：CI/CD、容器编排 | P2 | 部署、deployment |
| performance-engineer | 性能优化：监控、调优 | P2 | 性能、performance |

## 🎯 场景化深度参考

### 详细参考（引用）

**自然语言示例 + Fallback + 组合流** → 见 `references/quick-reference.md`

### 快速决策速查

```
多 Agent 协调   → context-manager
健康检查        → project-health-check
里程碑          → milestone-tracker
Agent 优化      → multi-agent-optimize
PRD            → create-prd
进度预测        → project-timeline-simulator
部署            → deployment-engineer
性能            → performance-engineer
未知任务        → context-manager + 询问澄清
```

---

## 🗣️ 自然语言触发示例（引用）

**详细示例** → 见 `references/quick-reference.md`

---

## ❓ Fallback 处理

**当任务无法匹配任何规则时**：

```markdown
1. 询问用户澄清：
   "这个任务是 Agent 协调、项目监控、还是其他？"

2. 如果用户无法描述：
   → context-manager（让协调核心帮你判断）
```

---

## 🔗 任务组合流

```
多 Agent 项目 → context-manager
             → multi-agent-optimize（如需优化）

项目监控 → project-health-check
         → milestone-tracker（里程碑）
         → project-timeline-simulator（预测）
```

---

## 🔗 与 gql-leader 主 skill 联动

当 Leader 角色加载 `leader-ext-skill` 时：

```markdown
1. 收到领导任务
2. 先加载 leader-ext-skill（路由器）
3. 根据任务路由到具体 skill
4. 执行完成后返回 context-manager 做协调
```

**注意**：`leader-ext-skill` 不会覆盖 `gql-leader` 主 skill，它们协同工作。

---

## 🚨 常见错误

### 错误 1: 过度路由

```
❌ "用户说协调 Agent，路由到 context-manager，
    然后又问用户要不要用 multi-agent-optimize"
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
✓  无法匹配时 → context-manager（让协调核心帮你判断）
```

---

## 🔗 相关角色联动

| 角色 | 协作场景 |
|------|---------|
| **arc** | 项目启动：leader-ext-skill 协调 → arc-ext-skill 设计架构 |
| **sm** | Sprint 管理：leader-ext-skill 监督 → sm-ext-skill 执行管理 |
| **coder** | 任务分配：leader-ext-skill 协调 → coder-ext-skill 执行开发 |
| **review** | 质量控制：leader-ext-skill 监督 → review-ext-skill 评审 |

---

## 🗣️ 示例对话

### 示例 1: 路由到 context-manager

```
用户：协调多个 Agent 工作
AI：分析：包含"协调"
     路由到 context-manager
     执行：多 Agent 协调
```

### 示例 2: 路由到项目监控

```
用户：检查项目健康状态
AI：分析：包含"健康"
     路由到 project-health-check
     执行：项目健康检查
```

### 示例 3: Fallback 处理

```
用户：帮我看看这个任务
AI：分析：包含"任务"
     但无法确定具体类型
     Fallback → context-manager
     执行：协调核心判断任务类型
     反馈：这个任务是"里程碑追踪"，建议用 milestone-tracker
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
REFS=/home/gql/repos/leader-ext-skill/references

mkdir -p $REFS

# P0 Skills
cp $BASE/agent-orchestration/agents/context-manager.md $REFS/context-manager.md

# P1 Skills
cp $BASE/commands-project-task-management/commands/project-health-check.md $REFS/project-health-check.md
cp $BASE/commands-project-task-management/commands/milestone-tracker.md $REFS/milestone-tracker.md
cp $BASE/agent-orchestration/commands/multi-agent-optimize.md $REFS/multi-agent-optimize.md
cp $BASE/commands-project-task-management/commands/create-prd.md $REFS/create-prd.md

# P2 Skills
cp $BASE/commands-project-task-management/commands/project-timeline-simulator.md $REFS/project-timeline-simulator.md
cp $BASE/cicd-automation/agents/deployment-engineer.md $REFS/deployment-engineer.md
cp $BASE/observability-monitoring/agents/performance-engineer.md $REFS/performance-engineer.md
```

### 7.3 references 添加 TL;DR

```bash
for f in references/*.md; do
  if ! grep -q "^<!-- TL;DR" "$f"; then
    case "$f" in
      context-manager.md)
        echo "<!-- TL;DR: 多Agent上下文协调：跨Agent状态共享、冲突检测 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      project-health-check.md)
        echo "<!-- TL;DR: 项目健康监控：进度、风险、阻塞 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      milestone-tracker.md)
        echo "<!-- TL;DR: 里程碑追踪：进度监控、报告 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      multi-agent-optimize.md)
        echo "<!-- TL;DR: 多Agent性能优化：调度、负载均衡 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      create-prd.md)
        echo "<!-- TL;DR: PRD模板化：PRD文档标准化 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      project-timeline-simulator.md)
        echo "<!-- TL;DR: 预测分析：Burndown、进度预测 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      deployment-engineer.md)
        echo "<!-- TL;DR: 部署编排：CI/CD、容器编排 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
      performance-engineer.md)
        echo "<!-- TL;DR: 性能优化：监控、调优 -->" | cat - "$f" > temp && mv temp "$f"
        ;;
    esac
  fi
done
```

---

## Step 8: 创建 learns/ 踩坑记录

**learns/README.md**：

```markdown
# Leader Ext Skill 踩坑沉淀

## 🏷️ 按标签索引

## #路径确认

### context-manager
- **位置**: `external_plugins/agent-orchestration/agents/context-manager.md`
- **注意**: 在 agents 目录

### multi-agent-optimize
- **位置**: `external_plugins/agent-orchestration/commands/multi-agent-optimize.md`
- **注意**: 在 commands 目录，不是 agents！profile_role_skill.md 中可能写错

### project-health-check / milestone-tracker / create-prd / project-timeline-simulator
- **位置**: `external_plugins/commands-project-task-management/commands/`

### deployment-engineer
- **位置**: `external_plugins/cicd-automation/agents/deployment-engineer.md`

### performance-engineer
- **位置**: `external_plugins/observability-monitoring/agents/performance-engineer.md`

## #source-区分

| 目录 | Skills |
|------|--------|
| `external_plugins/agent-orchestration/agents/` | context-manager |
| `external_plugins/agent-orchestration/commands/` | multi-agent-optimize |
| `external_plugins/commands-project-task-management/commands/` | project-health-check, milestone-tracker, create-prd, project-timeline-simulator |
| `external_plugins/cicd-automation/agents/` | deployment-engineer |
| `external_plugins/observability-monitoring/agents/` | performance-engineer |

## #路径陷阱

### multi-agent-optimize 路径问题
- **现象**: profile_role_skill.md 中写的是 `agent-orchestration/agents/multi-agent-optimize.md`
- **实际**: 实际路径是 `agent-orchestration/commands/multi-agent-optimize.md`
- **原因**: profile_role_skill.md 写错了
- **解决**: 使用 find 命令确认实际路径
```
find /home/gql/tmp/codebuddy-skills -name "*multi-agent*"
# 结果: external_plugins/agent-orchestration/commands/multi-agent-optimize.md
```
```

---

## Step 9: 创建 references/quick-reference.md

```bash
cat > /home/gql/repos/leader-ext-skill/references/quick-reference.md << 'EOF'
<!-- TL;DR: 自然语言示例 + Fallback + 组合流快速参考 -->

# Leader 技能路由快速参考

> 详细决策树见 SKILL.md 主文件

## 🗣️ 自然语言触发示例

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "协调多个 Agent" | context-manager | 多 Agent 协调 |
| "检查项目健康" | project-health-check | 健康检查 |
| "里程碑进度" | milestone-tracker | 里程碑追踪 |
| "优化 Agent 性能" | multi-agent-optimize | Agent 优化 |
| "写 PRD" | create-prd | PRD 文档 |
| "预测进度" | project-timeline-simulator | 进度预测 |
| "部署上线" | deployment-engineer | 部署 |
| "性能调优" | performance-engineer | 性能 |

---

## 🔄 Fallback 处理

当任务**无法匹配**以上任何规则时：

```
未知任务
    │
    ├─ 询问用户澄清：
    │   "这个任务是 Agent 协调、项目监控、还是其他？"
    │
    └─ 如果用户无法描述：
        └─→ context-manager（让协调核心帮你判断）
```

**Fallback 规则**：
```markdown
无匹配时 → context-manager → 让他判断用哪个 skill
```

---

## 🔗 任务组合流

### 组合 1: 多 Agent 项目管理

```
"管理多 Agent 项目"
    │
    ├─ context-manager（协调）
    │     └─ multi-agent-optimize（如需优化）
    │
    └─ project-health-check（健康检查）
```

### 组合 2: 项目监控

```
"监控项目状态"
    │
    ├─ project-health-check（健康检查）
    ├─ milestone-tracker（里程碑）
    └─ project-timeline-simulator（预测）
```

### 组合 3: 部署上线

```
"部署上线"
    │
    ├─ deployment-engineer（部署）
    └─ performance-engineer（性能验证）
```

---

## ⚡ 快速决策速查卡

```
┌─────────────────────────────────────────────────────────────┐
│  任务类型              │  首选 Skill         │  辅助      │
├─────────────────────────────────────────────────────────────┤
│  多 Agent 协调        │  context-manager    │            │
│  健康检查              │  project-health-c. │            │
│  里程碑追踪            │  milestone-tracker  │            │
│  Agent 优化            │  multi-agent-opt.. │            │
│  PRD 文档              │  create-prd        │            │
│  进度预测              │  project-timeline..│            │
│  部署                  │  deployment-eng... │            │
│  性能                  │  performance-eng.. │            │
│  未知任务              │  context-manager   │  询问澄清  │
└─────────────────────────────────────────────────────────────┘
```
EOF
```

---

## Step 10: 创建 update_readme.md 升级方案

```bash
cat > /home/gql/repos/leader-ext-skill/update_readme.md << 'EOF'
# Leader Ext Skill 升级方案

## 执行计划

详见 `leader-ext-skill-执行计划.md`（详细步骤说明）

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
REFS=/home/gql/repos/leader-ext-skill/references

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
cd /home/gql/repos/leader-ext-skill
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
| context-manager | `external_plugins/agent-orchestration/agents/context-manager.md` |
| multi-agent-optimize | `external_plugins/agent-orchestration/commands/multi-agent-optimize.md` |
| project-health-check | `external_plugins/commands-project-task-management/commands/project-health-check.md` |
| milestone-tracker | `external_plugins/commands-project-task-management/commands/milestone-tracker.md` |
| create-prd | `external_plugins/commands-project-task-management/commands/create-prd.md` |
| project-timeline-simulator | `external_plugins/commands-project-task-management/commands/project-timeline-simulator.md` |
| deployment-engineer | `external_plugins/cicd-automation/agents/deployment-engineer.md` |
| performance-engineer | `external_plugins/observability-monitoring/agents/performance-engineer.md` |
EOF
```

---

## Step 11: 创建 README.md

```bash
cat > /home/gql/repos/leader-ext-skill/README.md << 'EOF'
# Leader Ext Skill

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Platforms](https://img.shields.io/badge/platforms-hermes-blue.svg)](#)
[![Version](https://img.shields.io/badge/Version-1.0.0-green.svg)](SKILL.md)
[![Leader Skills](https://img.shields.io/badge/Leader_Skills-8-orange.svg)](#)
[![Auto Route](https://img.shields.io/badge/Auto_Route-Enabled-blue.svg)](#)

Leader 技能索引路由器 - 接收任何领导任务，智能推荐最合适的 skill 并执行。

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
| context-manager | 多 Agent 协调 | P0 |
| project-health-check | 项目健康监控 | P1 |
| milestone-tracker | 里程碑追踪 | P1 |
| multi-agent-optimize | 多 Agent 优化 | P1 |
| create-prd | PRD 模板化 | P1 |
| project-timeline-simulator | 进度预测 | P2 |
| deployment-engineer | 部署编排 | P2 |
| performance-engineer | 性能优化 | P2 |

## 工作流

详见 [SKILL.md](SKILL.md)

## 升级

详见 [update_readme.md](update_readme.md)
EOF
```

---

## Step 12: Git 提交

```bash
cd /home/gql/repos/leader-ext-skill
git add -A
git commit -m "feat: initial leader-ext-skill with all Leader skills"
git push origin main
```

---

## 验证清单

- [ ] 下载解压成功
- [ ] GitHub 仓库已创建/更新
- [ ] 所有 8 个 skill 路径验证通过
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
