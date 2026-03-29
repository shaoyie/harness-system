# REVIEWS

本目录存放 Reviewer / Tester 的独立评审记录。

## 规则
- 需要独立审查的任务，评审结论必须写入本目录
- 文件名格式：`<TASK-ID>-review-<YYYY-MM-DD>.md`
- 评审至少包含：范围检查、证据检查、风险发现、必需返工项、结论
- 若证据仅存在于聊天而未落盘，应判定为证据不足
- **评审文件不存在 = 评审未完成**，无论回复中描述了什么

---

## 评审文件模板

```markdown
# Review: <TASK-ID> — <YYYY-MM-DD>

## Metadata
- Task ID:
- PR:
- Reviewer:
- Verdict: Pass | Rework | Blocked
- Reviewed At: <YYYY-MM-DD HH:MM UTC+8>

## Scope Check
- [ ] 变更范围与任务卡 In Scope 一致
- [ ] 无未授权的范围扩展
- 说明：

## Evidence Check
| 验收标准 | 满足? | 依据 |
|----------|-------|------|
| 标准 1   | ✅/❌ |      |
| 标准 2   | ✅/❌ |      |

## Risks Found
- P0（阻塞）：
- P1（高）：
- P2（中）：

## Required Rework
- 若 Verdict = Pass：无
- 若 Verdict = Rework：（逐条列出，每条可验证）

## Rule / Test Suggestions
- （发现的流程缺口或改进建议）

## 关联文件更新
- [ ] 任务卡已更新（关联评审记录字段）
- [ ] PR 已写评审评论
- [ ] reviewer-to-planner handoff 已填写（若 Pass）
```
