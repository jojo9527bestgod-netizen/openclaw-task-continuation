# 下一步：自动闭环落地建议

当前已打通：
- main → monkey 执行链
- main → assi 监督链
- assi 形成续推指令
- monkey 按续推指令继续执行并回写状态

下一步若继续实现真正“自动闭环”，建议：

1. 由 `assi` 在 heartbeat 中识别 stale 任务
2. 由 `main` 作为转发器，把 assi 生成的续推指令真正投递给 `continue_target`
3. 执行 agent 完成后回写任务状态
4. assi 下轮 heartbeat 再次检查

这样可以先稳定跑通，不必一上来让 assi 直接跨 session 发命令。
