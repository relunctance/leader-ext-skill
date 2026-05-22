<!-- TL;DR: 自然语言示例 + Fallback + 组合流快速参考 -->

# Leader 技能路由快速参考

> 详细决策树见 SKILL.md 主文件

## 🗣️ 自然语言触发示例

| 用户实际说法 | → 路由到 | 说明 |
|-------------|---------|------|
| "管理上下文" | context-manager | 上下文共享 |
| "多 Agent 协作" | multi-agent-optimize | 协作优化 |
| "创建新 Agent" | create-buddy-agent | Agent 创建 |
| "hawk 评测" | hawk-eval | 模型评测 |
| "hawk 项目" | hawk-memory-dev | hawk 开发 |
| "项目架构" | project-architect | 技术选型 |

---

## 🔄 Fallback 处理

当任务**无法匹配**时：

```
无匹配 → context-manager（让上下文管理帮你判断）
```

---

## 🔗 任务组合流

### 组合 1: 新项目启动

```
"启动新项目"
    ├─ project-architect（项目架构）
    └─ create-buddy-agent（如需创建 Agent）
```

### 组合 2: 多 Agent 协作

```
"协调多个 Agent"
    └─ multi-agent-optimize
          ├─ 任务分配
          ├─ 开工启动
          └─ 进度监控
```

### 组合 3: 上下文同步

```
"同步团队上下文"
    └─ context-manager
          ├─ 上下文快照
          ├─ 状态同步
          └─ 变更通知
```

---

## ⚡ 快速决策速查卡

```
┌─────────────────────────────────────────────────────────────┐
│  场景              │  首选 Skill           │  组合        │
├─────────────────────────────────────────────────────────────┤
│  多 Agent 协作     │  multi-agent-optimize │              │
│  上下文管理        │  context-manager     │              │
│  创建新 Agent      │  create-buddy-agent  │              │
│  hawk 开发         │  hawk-memory-dev     │              │
│  hawk 评测         │  hawk-eval          │              │
│  项目架构          │  project-architect   │              │
│  任务分配          │  multi-agent-optimize │              │
│  进度跟踪          │  context-manager     │              │
│  阻塞处理          │  multi-agent-optimize │              │
│  未知              │  context-manager     │  询问澄清    │
└─────────────────────────────────────────────────────────────┘
```
