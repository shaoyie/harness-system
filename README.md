# Harness System（通用 AI 项目推进体系）

目标：让 AI（LLM）+ OpenClaw 在**长周期、多轮次、多角色**协作中持续、高质量、自动化推进项目，并保持可追踪、可审计、可回滚。

本体系吸收了你给出的几类实践（Anthropic long-running harness、OpenAI harness engineering、社区实践）并做了工程化落地。

---

## 1) 核心思想（先约束，再提速）

1. **Harness 是“轨道”，不是“提示词”**  
   Prompt 解决“这一轮怎么做”；Harness 解决“100 轮怎么不跑偏”。

2. **状态外置**  
   不信模型短期记忆，关键信息全部落盘（任务状态、决策记录、验证结果、风险）。

3. **双环路**  
   - 交付环：Plan → Build → Verify → Review → Merge
   - 自省环：复盘失败/返工原因，更新规则和模板

4. **质量左移 + 自动守门**  
   架构约束、lint、测试、契约校验、发布检查作为默认门禁。

5. **人类只做高杠杆决策**  
   人负责边界、优先级、关键取舍；AI 负责高频执行与证据沉淀。

---

## 2) 建议目录结构（可复制到任意项目）

```text
.harness/
  GOVERNANCE.md              # 规则总纲（不可破坏的边界）
  BACKLOG.md                 # 全局需求池（优先级 + 状态）
  ROADMAP.md                 # 里程碑视图
  DECISIONS/
    ADR-0001-*.md            # 架构决策记录
  TASKS/
    T-YYYYMMDD-001-*.md      # 单任务卡（输入/输出/验收）
  RUNS/
    YYYY-MM-DD/
      session-*.md           # 每轮执行日志（命令、结果、证据）
  REVIEWS/
    review-*.md              # 评审结论与返工建议
  METRICS/
    delivery-metrics.md      # 周期性指标与趋势
  TEMPLATES/
    *.md                     # 模板库（本目录 templates）
```

> 设计原则：**单一事实源（SSOT）+ 命名标准化 + 可脚本解析**。

---

## 3) 三层执行模型

- **L0 治理层（稳定）**：规则、质量红线、升级条件（人工介入点）
- **L1 编排层（半稳定）**：任务拆分、路由、状态机、重试策略
- **L2 执行层（高频）**：编码、测试、文档、评审、修复

OpenClaw 中建议映射：
- Manager（阿瞒）：优先级/资源/里程碑
- Architecture（贾诩）：任务建模、约束、验收设计
- Developer（赵子龙）：实现与单测
- Tester（小乔）：验证、回归、风险报告

---

## 4) 最小可运行流程（MVP）

1. 建立 `.harness/GOVERNANCE.md` + `BACKLOG.md`
2. 从 BACKLOG 拆出 3~5 个可独立交付的任务卡
3. 每个任务按模板执行：Plan → Build → Verify → Review
4. 每次会话必须写 `RUNS/日期/session-*.md`
5. 每周更新 `METRICS/delivery-metrics.md`
6. 每周至少一次“规则复盘”：把返工原因写回 GOVERNANCE/TEMPLATES

---

## 5) 你现在就可以做的第一步

- 先落地 `playbooks/project-adoption.md`
- 在项目仓库中创建 `.harness/` 目录并初始化基础文档
- 用 1 个真实需求跑完整闭环（不是演示任务）

详见：
- `00-principles.md`
- `01-operating-model.md`
- `03-task-lifecycle.md`
- `playbooks/project-adoption.md`
