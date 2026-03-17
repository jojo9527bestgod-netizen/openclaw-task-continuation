# 任务看板（最小可用版）

这个目录用于解决一个老毛病：**长任务做一会儿就停，必须等用户追问才继续。**

## 文件

- `active-tasks.json`：当前活跃任务总表
- `task-template.json`：新任务模板示例
- `workflow.md`：任务分工与续跑规则

## 适用场景

把下面这类都当成长任务：

- 预计超过 15 分钟
- 需要多阶段产出
- 需要「搜集 → 整理 → 写作 / 汇总 / 改写」
- 做一轮通常不可能直接结束

例如：

- 写 PPT
- 写一批小红书文案
- 收集资料并整理报告
- 多轮修订内容

## 基本流程

1. `main` 接到任务
2. 把任务登记到 `active-tasks.json`
3. 指派 owner agent（通常是 `monkey` 或 `panda`）
4. owner agent 每完成一个阶段就更新状态
5. `assi` 定期检查：没完成、又长时间没动，就推动继续
6. `main` 负责最终对用户汇总

## 字段说明

每个任务至少应包含：

- `task_id`：唯一 ID，建议 `task-YYYYMMDD-001`
- `title`：任务标题
- `owner_agent`：负责执行的 agent，如 `monkey`
- `status`：`todo` / `running` / `blocked` / `paused` / `done`
- `current_step`：当前阶段
- `last_progress_at`：最近一次推进时间（ISO 8601）
- `next_action`：下一步要做什么
- `report_to`：汇报对象，默认 `main`
- `summary`：到当前为止完成了什么
- `needs_user_input`：是否卡在等用户决定
- `user_block_reason`：如果在等用户，写清原因

## 规则

- 长任务不要试图一轮做完。
- 每轮只推进一小段。
- 每完成一段，就更新状态。
- 如果任务未完成，不要默认消失。
- 只有在 `done`、`paused` 或 `needs_user_input=true` 时，监督强度才下降。

## 第一步先求稳

先手工维护这个 JSON，先把流程跑顺。
后面如果顺手，再考虑做自动化写入或更复杂的编排。
