# 03 - Task Lifecycle（任务生命周期）

## 标准任务卡字段（必须）
- Task ID
- Issue / PR / Branch 元数据
- 背景与目标
- 范围（In/Out）
- 输入依赖
- 实施步骤
- 验收标准（可机器验证优先）
- 风险与回滚
- 产出清单
- 状态历史（含时间戳）

## 共享协作面与文件面的分工
- **Issue**：任务认领、互斥锁、全局可见状态、assignee、超时恢复锚点
- **PR**：候选变更与 pre-merge review 入口
- **Task / Run Log / Review 文件**：结构化语义、证据、验收、返工原因

原则：
1. 任务认领与状态共享不能只靠 task branch，必须通过共享可见面（Issue / PR）。
2. 主分支保留已生效状态；candidate branch 保留候选执行态与候选评审态。
3. develop / review 开始前，必须先同步目标分支到最新远端状态。

## 推荐状态流转（当前精简版）
默认主流程：
`status:new -> status:needs-design -> status:ready -> status:in-progress -> status:review -> status:ready-for-merge -> status:done`

返工 / 异常支路：
- 任意阶段可进入 `status:blocked`
- `status:review` 发现问题或 CI 失败时进入 `status:changes-required`
- `status:changes-required` 修复后回到 `status:in-progress`

预留扩展状态（按需启用，不是当前默认必经阶段）：
- `status:triaging`
- `status:designing`
- `status:ready-for-test`
- `status:testing`
- `status:deployed`

## 执行节奏（每轮）
1. **Plan**：确认范围、补齐缺失上下文、写清 Issue/Task 基线
2. **Build**：小步提交，避免一次性大改
3. **Verify**：运行本地验证与 CI 所需检查
4. **Review**：基于 PR / candidate branch 进行 pre-merge review
5. **Record**：更新 issue 状态、任务卡、session 日志、评审记录

## 角色动作要求
### Builder
- 开工前：fetch/pull 基线分支与工作分支最新远端状态
- 认领任务时：更新 Issue assignee / status / branch 信息
- 提交 review 前：
  - push candidate branch
  - 创建/更新 PR
  - 回写 task/run log/handoff
  - 将 issue 状态切到 `status:review`

### Reviewer
- review 前：fetch 最新远端、checkout 对应 PR / branch、确认待审 commit
- review 对象：candidate branch，而不是默认主分支
- review 结论：写回 PR，同时写入 candidate branch 的 review 文件
- review 不通过：将 issue 状态切到 `status:changes-required`
- review 通过且 required CI checks 通过：切到 `status:ready-for-merge`

## 状态时间戳与恢复
每次状态变更都必须保留：
- 变更时间（带时区）
- 变更人 / agent
- 从什么状态变到什么状态
- 对应 issue / PR / commit

这些信息至少应出现在：
1. issue / PR comment 或等价共享可见面
2. 任务卡中的 `Status History` 区块

## Definition of Done（DoD）
- 代码已合并并可复现构建
- required CI checks 通过
- review 已通过，且无未处理阻塞意见
- 文档更新（README / 接口 / 迁移说明）
- 任务卡状态更新为 `status:done`
- 指标记录已更新（Lead Time、返工次数等）

## 常见反模式
- “看起来完成了”但无证据
- 未更新 issue 状态，导致并发冲突或重复认领
- reviewer 未同步远端就开始审查，导致审错提交
- 把候选分支中的状态误当成主分支共享状态
- 不写回决策与经验，重复踩坑
