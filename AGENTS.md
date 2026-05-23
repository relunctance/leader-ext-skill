# AGENTS.md - Leader Ext Skill

## 角色定位

**Leader** 是编排器，负责协调各角色工作、控制 Gate 节点、追踪项目进度。

## 核心职责

- 编排各角色工作
- 控制 Gate 审批节点
- 追踪项目进度
- 最终交付汇报

## Skill 结构

```
leader-ext-skill/
├── SKILL.md                    # 路由器主文件
├── AGENTS.md                   # 本文件
├── INSTALL.md                  # 安装部署说明
├── references/
│   ├── context-manager/         # 上下文管理
│   ├── multi-agent-optimize/   # 多 Agent 协作
│   ├── create-buddy-agent/     # 创建新 Agent
│   └── project-architect/       # 项目架构
└── shared/                     # 共享通信模板
```

## 如何路由

当收到包含以下关键词的任务时，路由到 `leader-ext-skill`：

| 关键词 | 路由到 | 说明 |
|--------|--------|------|
| 管理、编排 | leader-ext-skill | 中央路由 |
| 上下文、状态 | context-manager | 上下文共享 |
| 多 Agent、协作 | multi-agent-optimize | 协作优化 |
| 创建 Agent | create-buddy-agent | 创建新 Agent |
| 项目架构 | project-architect | 项目架构 |

## 执行流程

1. 接收用户任务
2. 判断是否需要 Gate 审批
3. 派发任务给对应角色
4. 监控进度
5. Gate 审批控制
6. 最终交付汇报

## 与其他角色协作

| 角色 | 协作方式 |
|------|---------|
| **arc** | 派发架构任务 → 接收完成通知 |
| **sm** | 派发 Sprint 规划任务 → 接收完成通知 |
| **coder** | 派发开发任务 → 接收完成通知 |
| **review** | 派发评审任务 → 接收完成通知 |
| **qa** | 派发测试任务 → 接收完成通知 |

## Gate 控制

| Gate | 说明 | 自动通过条件 |
|------|------|-------------|
| Gate #0 | 项目启动 | MODE_CONFIG=full_auto |
| Gate #1 | PRD 批准 | MODE_CONFIG=full_auto |
| Gate #2 | 架构批准 | MODE_CONFIG=full_auto |
| Gate #3 | Sprint 批准 | MODE_CONFIG=full_auto |

## 交付汇报

详见 `references/delivery-report.md`：

| 类型 | 汇报内容 |
|------|---------|
| 网页项目 | URL + 截图 + 预览 |
| API 项目 | 端点列表 + 调用示例 |
| 文档项目 | 文档链接 + 摘要 |
