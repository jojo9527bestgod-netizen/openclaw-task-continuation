# assi 自动续推作战手册

## 目标

发现长任务 stale 后，不只是汇报，还要形成下一轮续推动作。

## 看板路径

`/Users/dora/.openclaw/workspace/tasks/active-tasks.json`

## 关注字段

- `status`
- `needs_user_input`
- `last_progress_at`
- `stale_after_minutes`
- `continue_target`
- `continue_brief`
- `auto_continue_count`
- `max_auto_continues`

## 什么时候触发续推

满足以下条件才触发：

- `status = running`
- `continuation_mode = supervised`
- `needs_user_input != true`
- 已超过 `stale_after_minutes`
- `auto_continue_count < max_auto_continues`

## 你的输出分两层

### 对用户
用一句短话说明：
- 哪个任务还在跑
- 已完成到哪
- 下一步准备推动什么

### 对执行 agent（例如 monkey）
生成一条短续推指令：

继续执行任务「<title>」：<continue_brief>

## 第一版约束

- 先别编造“已经自动投递成功”
- 先形成续推指令和监督消息
- 真正投递仍可由 main 作为转发器
- 每次优先只盯 1 个最该续推的任务
