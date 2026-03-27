# 06 - Prompt Design（Prompt 设计落地）

## 核心原则

### 1. Prompt 不承载项目全部知识
Prompt 只负责：
- 角色定位
- 行为边界
- 输出结构

项目知识应优先写入：
- GOVERNANCE.md
- TASKS/
- RUNS/
- DECISIONS/
- README/docs

### 2. 使用三层结构

## Layer A: Shared Base
所有角色共享的约束：
- 状态外置
- 不擅自扩范围
- 必须提供证据
- 关键变更升级处理

## Layer B: Role Prompt
角色差异：
- Planner：边界、验收、拆分
- Builder：实现、验证、记录
- Reviewer：独立审查、问题发现

## Layer C: Task Context
每轮动态注入：
- 当前任务卡
- 最近运行日志
- 相关代码/文档
- 当前 diff 或目标

### 3. 控制 Prompt 长度
- 共享规则尽量稳定
- 任务信息通过文件注入
- 避免不断向系统 prompt 累加“历史教训”
- 高频经验沉淀进模板和治理文件，而不是变成一长串自然语言提示

## 一个推荐格式

```text
[Shared Base]
...

[Role Prompt]
...

[Task Context]
Task ID: ...
Goal: ...
Files: ...
Acceptance: ...
Last run: ...
```

## 常见错误
- 把项目 wiki 全塞进 prompt
- 一个 prompt 同时要求规划、实现、审查、发布
- 任务上下文不结构化，导致 agent 每轮重新理解
- 经验不落盘，只在 prompt 里反复追加

## 实操建议
- Shared Base 保持 10~25 行
- Role Prompt 保持 10~25 行
- Task Context 尽量结构化，按任务最小化注入
- 每次失败复盘后，优先更新规则或模板，再考虑改 prompt
