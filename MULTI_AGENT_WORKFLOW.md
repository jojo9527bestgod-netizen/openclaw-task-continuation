# MULTI_AGENT_WORKFLOW.md

## 目标

这套工作流用于解决一个核心问题：

**长任务不要做一会儿就停；任务要能被监督、续推、回写状态，而不是等用户追问。**

## 当前架构

4 个 agent 分工：

- `main`：总控 / 建任务 / 汇总 / 续推转发
- `monkey`：内容执行
- `panda`：信息搜集
- `assi`：监督 / stale 判断 / 续推意图生成

## 当前已打通

### 已完成
- subagent 默认模型已切到 `minimax-cn/MiniMax-M2.5`
- `monkey` 子任务链路已验证：
  - 能拉起
  - 能读文件
  - 能写文件
  - 能回写共享任务看板
- `assi` 监督链路已验证：
  - 能读看板
  - 能识别 running 任务
  - 能生成监督消息
  - 能形成续推指令
- `main` 续推转发器规则已落地
- 共享任务看板已支持 supervised continuation 字段

### 当前结论
这套系统已经具备“执行 + 监督 + 续推意图 + 状态回写”的最小闭环。

## 共享任务看板

主文件：`tasks/active-tasks.json`

相关文档：
- `tasks/README.md`
- `tasks/workflow.md`
- `tasks/continuation-spec.md`
- `tasks/router-example.md`
- `tasks/task-template.json`
- `tasks/task-router-template.json`

## 关键规则入口

- 总览：`context/agent-orchestration-overview.md`
- main 转发器：`context/main-continuation-router.md`
- 下一步自动闭环建议：`context/next-step-auto-loop.md`
- assi 自动续推手册：`/Users/dora/.openclaw/workspace-assi/context/auto-continue-playbook.md`
- monkey 内容角色：`/Users/dora/.openclaw/workspace-monkey/context/content-role.md`
- panda 信息角色：`/Users/dora/.openclaw/workspace-panda/context/research-role.md`

## 当前仍未完成（很重要）

### 自动触发版：**还没完成**
当前虽然已经打通：
- assi 识别 stale
- assi 形成续推指令
- main 可转发给 monkey
- monkey 可继续执行并回写

但是：

**“自动触发版” 还没有真正挂进 heartbeat / cron / 调度链。**

也就是说，当前系统已经可用，但还不是完全无人值守版。

## 推荐下一步

优先完成：
1. 把 `assi → main → monkey` 的续推链接进实际 heartbeat / cron
2. 让 stale 检测可以定时触发
3. 让 main 在满足条件时自动转发下一轮执行

## /new 后如何恢复上下文

新会话启动后，若涉及多 agent 长任务编排，优先读取：

1. `MULTI_AGENT_WORKFLOW.md`
2. `context/agent-orchestration-overview.md`
3. `tasks/workflow.md`
4. `tasks/continuation-spec.md`
5. `context/main-continuation-router.md`

## Git / 保存建议

这套工作流应保存在版本控制中。推送到 GitHub 前，先检查是否包含敏感信息或本地路径中不该公开的内容。
