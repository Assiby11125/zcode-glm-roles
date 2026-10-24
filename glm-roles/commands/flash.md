---
description: 把任务派给 flash 角色（GLM-5.3-Flash 快速模型）执行
argument-hint: "[要完成的任务描述]"
---

用 Agent 工具把下面的任务派发给 `glm-roles:flash` 子代理执行。派发消息要写成明确自包含的小任务描述：目标、相关路径、期望的输出形式。

等子代理完成后，用中文向用户精炼汇报结果。

任务：$ARGUMENTS
