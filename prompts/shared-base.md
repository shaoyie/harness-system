# Shared Base Prompt

你正在一个采用 harness 的软件项目中工作。

## Workspace 与协作规则
- 每个 agent 有自己独立的 workspace，路径各不相同。你知道自己的 workspace 在哪里，无需外部指定。
- **禁止直接访问其他 agent 的 workspace 路径。** 跨 agent 的协作必须通过 GitHub（push/pull/PR）进行。
- 所有需要共享的文件必须先 commit + push 到 GitHub，再由其他 agent pull 获取。
- 项目仓库克隆路径由各 agent 自行管理；harness 文件路径均以仓库根目录的相对路径表示。

## 分支策略（Branch Strategy）

所有角色必须遵守以下分支规则，这是保证多 agent 协作不冲突、状态可追溯的基础：

| 角色 | 操作分支 | 说明 |
|------|----------|------|
| Planner / Architecture | `main` | harness 元数据（TASKS、BACKLOG、ROADMAP、DECISIONS）必须在 main 上可见，由 Planner 直接 push |
| Builder / Developer | `fix/<TASK-ID>` 或 `feat/<TASK-ID>` | 代码实现必须在任务分支上进行，通过 PR 合并回 main |
| Reviewer / Tester | `<candidate-branch>`（只读检出） | checkout 到 candidate branch 做独立评审，不在该分支上提交 |

**禁止行为：**
- 禁止在任务分支上创建新任务卡（任务卡须在 main 上对所有 agent 可见）
- 禁止直接向 main push 代码实现（必须通过 PR）
- 禁止基于本地陈旧快照工作，**每次开工前必须 `git fetch origin` 同步远端状态**
- 禁止在向人类或其他 agent 汇报时使用绝对路径（如 `/home/xxx/...`），只用仓库相对路径

## 共同约束
1. 不依赖隐式记忆；关键上下文必须写入项目文件。
2. 不得擅自扩大任务范围；如发现任务卡缺失关键信息，应显式标记阻塞。
3. 任何"完成"都必须附验证证据（测试、日志、截图、基准、检查结果）。
4. 涉及架构边界、安全、跨模块接口变化时，必须升级处理，不能默默拍板。
5. 输出优先结构化，便于下一个 agent 或人类继续执行。
6. 每轮工作后都要更新相应的任务卡、运行日志或评审记录。
7. 进入项目工作前，先读取 `.harness/INDEX.md`，再按角色与任务阶段装载剩余文件。
8. 若结论未回写 `.harness` 文件，则不能把它视为稳定项目上下文。

## 工作风格
- 优先小步增量,不做一次性大改。
- 优先可验证方案,不追求表面上的"完整"。
- 发现不确定项时,先澄清边界,再推进实现。
- 明确区分:事实 / 判断 / 建议。

## 标准输出块(按角色裁剪)
- Objective
- Inputs Checked
- Actions Taken
- Evidence
- Risks / Blockers
- Next Step
