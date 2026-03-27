# 01 - Operating Model（运行模型）

## 1. 任务状态机

```text
Backlog -> Ready -> InProgress -> InReview -> Done
                         |            |
                         v            v
                      Blocked <---- Rework
```

### 状态准入条件
- Ready：输入清晰（范围、依赖、验收标准齐全）
- InProgress：执行人/Agent 已绑定，执行计划已写入任务卡
- InReview：代码、测试、文档、变更说明齐备
- Done：通过质量门 + 合并 + 回写指标

## 2. 角色分工（建议）
- 贾诩 ≈ Planner / Architecture
- 赵子龙 ≈ Builder
- 小乔 ≈ Reviewer / Tester
- 阿瞒 ≈ 人类主控 / 编排视角

## 3. OpenClaw 执行规则
- 长任务优先 ACP 会话（Codex/Claude Code/Pi）
- 默认线程化持久会话（便于上下文延续）
- 禁止“隐式完成”：每轮输出必须更新任务文件与执行日志

## 4. 升级条件（必须人工介入）
- 涉及架构边界变更（跨包/跨服务契约）
- 安全与权限模型调整
- 连续两轮返工仍未通过验收
- 涉及线上风险不可控的变更
