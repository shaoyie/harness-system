# 04 - Quality Gates（质量门）

## 当前策略：质量左移 + 自动守门
默认将 lint、单测、契约校验、打包检查、CI 检查前移到 Builder 阶段完成；当前阶段 **testing 不是所有任务的默认必经门**，仅在高风险或 CI 无法覆盖时按需启用。

## G0 结构门（提交前）
- 目录与命名符合约定
- 任务卡字段完整
- Issue / Branch / PR 元数据存在
- 变更说明与运行日志存在

## G1 本地验证门（Builder 提交 review 前）
- lint / format checks 通过
- type check 通过（如适用）
- 受影响模块单测通过
- 契约 / schema / 打包检查通过（如适用）
- 本轮验证证据已写入 run log

## G2 CI 门（PR / 合并前）
- GitHub Actions required checks 通过
- 覆盖率不低于项目阈值（如有）
- 安全扫描无高危（如适用）
- 关键集成检查通过，或明确记录无法运行原因

## G3 评审门（合并前）
- reviewer 基于最新 candidate branch 完成审查
- 架构评审通过（涉及边界变更时）
- 风险与回滚方案可执行
- 文档更新齐备
- review 未留下阻塞性未解决意见

## G4 合并门
- issue 状态已更新为 `status:ready-for-merge`
- review 通过
- required CI checks 通过
- task / run log / review 记录已同步

## G5 运行门（发布前，按需启用）
- 发布检查清单通过
- 监控与告警已就绪
- 回滚演练可执行
- 仅在涉及部署时启用 `status:deployed` 等扩展状态
