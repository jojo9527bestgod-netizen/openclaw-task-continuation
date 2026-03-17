# 自动续推机制（最小正式版）

目标：让长任务在未完成、且不需要用户拍板时，能够被监督 agent 发现并继续推动，而不是等用户追问。

## 角色

- `main`：创建任务、汇总结果
- `monkey` / `panda`：执行任务
- `assi`：监督、判断 stale、生成续推动作

## 任务新增字段

建议每个长任务补这些字段：

- `continuation_mode`: `manual` | `supervised`
- `stale_after_minutes`: 超过多少分钟未更新就算停住
- `last_supervised_at`: 上次被监督检查/推动的时间
- `continue_target`: 续推目标 agent，如 `monkey`
- `continue_brief`: 下一轮续推时应执行的简短动作
- `max_auto_continues`: 最多自动续推多少轮，避免死循环
- `auto_continue_count`: 当前已自动续推次数

## stale 判定

当满足以下条件时，任务可视为 stale：

- `status = running`
- `needs_user_input != true`
- 当前时间 - `last_progress_at` >= `stale_after_minutes`
- `auto_continue_count < max_auto_continues`

## assi 的动作

当任务 stale 时，assi 应：

1. 读取任务的 `continue_target` 和 `continue_brief`
2. 生成一条非常短的续推指令
3. 更新：
   - `last_supervised_at`
   - `auto_continue_count += 1`
4. 对用户做一句阶段播报

## 续推指令模板

推荐：

继续执行任务「<title>」：<continue_brief>

例：

继续执行任务「小红书账号首批10条选题」：补足剩余选题到10条，并为每条补一句切入角度。

## 第一版边界

- 先允许 assi 形成和记录续推动作
- 真正的跨 agent 自动投递，可以继续沿用 main 作为转发器
- 等这套稳定后，再考虑让 assi 直接触发下一轮
