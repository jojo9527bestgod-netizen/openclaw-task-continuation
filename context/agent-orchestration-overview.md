# 4-Agent 协作总览（最小版）

## 分工

- `main`：接任务 / 登记任务 / 分派 / 汇总 / 续推转发
- `monkey`：内容创作执行
- `panda`：资料与信息搜集
- `assi`：监督、续跑判断、阶段汇报

## 共享状态

统一看板：`/Users/dora/.openclaw/workspace/tasks/active-tasks.json`

## 长任务规则

- 先登记，再执行
- 一次只推进一个小阶段
- 每阶段后更新状态
- 未完成任务不能静默消失
- 需要拍板时才打断用户

## 监督式续推（当前正式方案）

- `assi` 负责从看板中识别 stale 任务
- `assi` 负责形成续推意图 / 续推指令
- `main` 负责把续推指令真正转发给目标执行 agent
- `monkey` / `panda` 负责执行并回写状态

## 当前测试任务

- `task-20260317-001`
- 标题：小红书账号首批 10 条选题
- owner：`monkey`
- 监督：`assi`
- 转发：`main`
