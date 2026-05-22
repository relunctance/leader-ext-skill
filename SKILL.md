---
name: leader-ext-skill
description: Leader 技能索引路由器 - 接收任何管理任务，智能推荐最合适的 skill 并执行
version: 2.0.0
author: relunctance
license: MIT
category: gql-bots
tags:
  - leader
  - coordination
  - multi-agent
  - skill-router
  - gql-bots
  - intelligent-router
hermes:
  platforms:
    hermes: true
  auto_route: true
---

# Leader Ext Skill - 智能技能路由器

> **核心定位**：Leader 角色的中央路由器。任何管理/协调任务进来，先查这里，再路由到具体 skill。

---

## ⚡ TL;DR 速查索引

| 你要做的事 | 直接路由 | 说明 |
|------------|---------|------|
| 管理上下文 | context-manager | 上下文共享 |
| 多 Agent 协作 | multi-agent-optimize | 协作优化 |
| 创建 Agent | create-buddy-agent | 创建新 Agent |
| 项目架构 | project-architect | 项目架构 |
| 任务分配 | multi-agent-optimize | 任务分配 |
| 进度跟踪 | context-manager | 状态同步 |
| 不知道用哪个 | context-manager | 让它帮你判断 |

---

## ⚡ 快速路由（必读）

### 任务 → Skill 速查

| 你的任务（说人话） | → 推荐 Skill | 直接调用 |
|------------------|-------------|---------|
| "管理上下文" | context-manager | `hermes -p leader -s context-manager` |
| "多 Agent 协作" | multi-agent-optimize | `hermes -p leader -s multi-agent-optimize` |
| "创建新 Agent" | create-buddy-agent | `hermes -p leader -s create-buddy-agent` |

| "项目架构" | project-architect | `hermes -p leader -s project-architect` |

### 一句话触发规则（增强版）

```
任务包含...              → 直接路由到...
────────────────────────────────────────────────────────────────────────────
# 上下文管理
"上下文"、"context"、"共享" → context-manager
"状态同步"、"同步状态" → context-manager
"记忆"、"memory" → context-manager

# 多 Agent 协作
"多 Agent"、"multi-agent"、"协作" → multi-agent-optimize
"任务分配"、"分配任务" → multi-agent-optimize
"协调"、"coordinator" → multi-agent-optimize
"开工"、"暂停"、"恢复" → multi-agent-optimize

# Agent 创建
"创建 Agent"、"new agent" → create-buddy-agent
"新建角色"、"new role" → create-buddy-agent
"注册 Agent" → create-buddy-agent



# 项目架构
"项目架构"、"architecture" → project-architect
"项目设计"、"系统设计" → project-architect
"技术选型"、"技术决策" → project-architect

# 进度管理
"进度"、"sprint"、"冲刺" → context-manager
"看板"、"kanban"、"任务板" → context-manager
"阻塞"、"blocker" → multi-agent-optimize

# 团队管理
"团队"、"team"、"成员" → multi-agent-optimize
"评审"、"review"、"meeting" → context-manager
"计划"、"planning" → context-manager
```

---

## 🔀 智能路由决策树

```
收到管理任务
    │
    ├─ 🎯 上下文/状态管理？
    │   └─ context-manager
    │       ├─ 上下文共享
    │       ├─ 状态同步
    │       └─ 记忆管理
    │
    ├─ 🎯 多 Agent 协作？
    │   └─ multi-agent-optimize
    │       ├─ 任务分配
    │       ├─ 开工/暂停/恢复
    │       └─ 阻塞处理
    │
    ├─ 🎯 创建新 Agent？
    │   └─ create-buddy-agent
    │       ├─ Agent 设计
    │       └─ 注册配置
    │

    ├─ 🎯 项目架构？
    │   └─ project-architect
    │
    └─ ❓ 不知道
        └─ context-manager + 询问澄清
```

---

## 📋 技能地图

| Skill | TL;DR | 级别 | 触发关键词 |
|-------|-------|------|-----------|
| context-manager | 上下文管理：上下文共享、状态同步 | P0 | 上下文、状态同步、记忆 |
| multi-agent-optimize | 多 Agent 优化：任务分配、协作编排 | P0 | 多 Agent、协作、任务分配 |
| create-buddy-agent | Agent 创建：Agent 设计、注册配置 | P0 | 创建 Agent、新建角色 |

| project-architect | 项目架构：架构设计、技术选型 | P1 | 项目架构、技术选型 |

---

## 🎯 场景化深度参考（4大场景）

### 场景 1: 多 Agent 协作 🏗️

```
需求：协调多个 Agent 工作
    │
    └─ multi-agent-optimize
          ├─ 开工
          │     → 创建任务分配
          │     → 启动各 Agent
          │
          ├─ 监控
          │     → 进度跟踪
          │     → 阻塞识别
          │
          └─ 暂停/恢复
                → 任务切换
                → 状态保存
```

### 场景 2: 上下文管理 🔄

```
需求：管理团队上下文共享
    │
    └─ context-manager
          ├─ 上下文共享
          │     → 定期同步
          │     → 变更通知
          │
          ├─ 状态同步
          │     → 任务状态
          │     → 阻塞状态
          │
          └─ 记忆管理
                → 重要决策记录
                → 上下文快照
```

### 场景 3: 创建新 Agent 🔧

```
需求：创建一个新的 Agent
    │
    └─ create-buddy-agent
          ├─ 分析需求
          │     → 确定角色
          │     → 定义职责
          │
          ├─ 设计 Agent
          │     → 编写 skill
          │     → 配置权限
          │
          └─ 注册验证
                → 提交注册
                → 功能验证
```

### 快速决策速查

```
┌────────────────────────────────────────────────────────────┐
│  场景              │  路由顺序                              │
├────────────────────────────────────────────────────────────┤
│  多 Agent 协作     │  multi-agent-optimize                  │
│  上下文管理        │  context-manager                       │
│  创建新 Agent      │  create-buddy-agent                    │
│  项目架构          │  project-architect                     │
│  任务分配          │  multi-agent-optimize                  │
│  进度跟踪          │  context-manager                       │
│  阻塞处理          │  multi-agent-optimize                  │
│  未知任务          │  context-manager + 询问澄清           │
└────────────────────────────────────────────────────────────┘
```

---

## ❓ Fallback 处理

当任务**无法匹配**以上任何规则时：

```
未知任务
    │
    ├─ 询问用户澄清：
    │   "这个任务是上下文管理、多 Agent 协作、还是创建 Agent？"
    │
    └─ 如果用户无法描述：
        └─→ context-manager（让上下文管理帮你判断）
```

---

## 🔗 任务组合流

### 组合 1: 新项目启动

```
"启动新项目"
    │
    ├─ project-architect（项目架构）
    │     → 技术选型
    │     → 系统设计
    │
    └─ create-buddy-agent（如需创建 Agent）
          → Agent 设计
          → 任务分配
```

### 组合 2: 多 Agent 协作

```
"协调多个 Agent"
    │
    └─ multi-agent-optimize
          ├─ 任务分配
          ├─ 开工启动
          └─ 进度监控
```

### 组合 3: 上下文同步

```
"同步团队上下文"
    │
    └─ context-manager
          ├─ 上下文快照
          ├─ 状态同步
          └─ 变更通知
```

---

## 🔗 与 gql-leader 主 skill 联动

**注意**：`leader-ext-skill` 不会覆盖 `gql-leader` 主 skill，它们协同工作。

```
┌─────────────────────────────────────────────────────────────┐
│  gql-leader 主 skill                                        │
│    │                                                        │
│    ├─ 通用管理任务 → leader-ext-skill（路由）               │
│    │              └─→ 具体 skill 执行                         │
│    │                                                        │
│    └─ 特定技能任务 → 直接调用具体 skill                       │
└─────────────────────────────────────────────────────────────┘
```

**何时使用 leader-ext-skill**：
- 任务模糊，需要判断用哪个 skill
- 复杂任务需要多 skill 组合
- 不确定某个 skill 是否适用

**何时直接调用具体 skill**：
- 任务明确，比如"协调多个 Agent"
- 已确定需要哪个 skill
- 只需要单个 skill

---

## 📖 References 快速索引

详见 `references/quick-reference.md`（自然语言示例 + Fallback + 组合流）

每个 skill 文件都有 TL;DR 摘要：

| Skill | TL;DR | 说明 |
|-------|-------|------|
| context-manager.md | 上下文管理 | 共享、状态同步 |
| multi-agent-optimize.md | 多 Agent 优化 | 协作、任务分配 |
| create-buddy-agent.md | Agent 创建 | 设计、注册 |


---

## 🚨 常见错误

| 错误 | 正确做法 |
|------|---------|
| 直接说"协调" | 说明协调什么（Agent？团队？） |
| 不确定用哪个 skill | → context-manager 让它帮你判断 |
| 过度路由 | 直接路由到最可能的 skill |
| 忘记 Fallback | 无法匹配时 → context-manager |

---

## 🔗 相关角色联动

|| 角色 | 协作场景 |
|------|---------|
| **arc** | 项目启动：leader-ext-skill 规划 → arc-ext-skill 评审架构 |
| **coder** | 任务分配：leader-ext-skill 分配 → coder-ext-skill 执行开发 |
| **review** | 代码评审：leader-ext-skill 协调 → review-ext-skill 评审 |
| **sm** | Sprint 管理：leader-ext-skill 监督 → sm-ext-skill 执行规划 |
| **qa** | 测试协调：leader-ext-skill 协调 → qa-ext-skill 执行测试 |

---

## 📋 Kanban 调度指南

### 核心原则

**leader 调度其他角色的正确方式**：通过 `hermes kanban create` 创建任务 + `--skill gql-{role}` 指定 skill。

### 调度命令模板

```bash
# 1. 创建任务并指定角色 skill
hermes kanban create "【{角色}】{任务名称}" \
  --body "{任务描述}" \
  --assignee {角色} \
  --skill gql-{角色} \
  --workspace dir:{{PROJECT_DIR}} \
  --tenant {{PROJECT_NAME}} \
  --priority 1 \
  --max-retries 2

# 2. 立即订阅飞书群
hermes kanban notify-subscribe <task-id> --platform feishu --chat-id {{FEISHU_MAIN}}

# 3. 如有依赖，建立 link
hermes kanban link <parent-task-id> <child-task-id>
```

### 角色 Skill 对照表

|| 角色 | Skill 名称 | 说明 |
|------|---------|------|
| arc | `gql-arc` | 架构设计 |
| sm | `gql-sm` | Sprint 规划 |
| coder | `gql-coder` | 开发实施 |
| review | `gql-review` | 代码评审 |
| qa | `gql-qa` | 测试验证 |

### 完整调度流程

```
用户指令
    ↓
leader 规划任务
    ↓ 创建 Kanban 任务
arc/sm/coder/review/qa 执行
    ↓ 评论 + 飞书通知
leader 监控进度
    ↓ 审批/协调
下一个 Gate
```

### 常见问题

| 问题 | 解决 |
|------|------|
| worker 加载不了 skill？ | 检查 qa/coder 等 profile 的 skills/ 目录是否包含 gql-{role} |
| 任务创建后没反应？ | 检查是否执行了 `notify-subscribe` |
| 角色不执行任务？ | 检查 `--assignee` 是否与 skill 名称对应 |

---

## 🗣️ 示例对话

### 示例 1: 路由到 multi-agent-optimize

```
用户：协调多个 Agent 工作
AI：分析：包含"协调"、"多个 Agent"
     路由到 multi-agent-optimize
     执行：多 Agent 协作编排
```

### 示例 2: 路由到 create-buddy-agent

```
用户：创建一个新的 coder Agent
AI：分析：包含"创建"、"coder Agent"
     路由到 create-buddy-agent
     执行：Agent 创建流程
```

### 示例 3: Fallback 处理

```
用户：帮我管理一下团队
AI：分析：包含"管理"、"团队"
     但无法确定具体类型
     Fallback → context-manager
     执行：上下文管理判断任务类型
     反馈：这个任务是"多 Agent 协作"，建议用 multi-agent-optimize
```

---

## 升级说明

查看 [update_readme.md](update_readme.md) 了解如何同步最新 skill。

当前版本：v2.0.0
