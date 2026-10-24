---
description: 列出 glm-roles 插件的角色与模型映射
---

直接向用户介绍当前可用的角色（不要派发子代理）：

| 角色 | 子代理类型 | 模型 | 适用任务 |
|------|-----------|------|----------|
| deep | `glm-roles:deep` | GLM-5.3（thoughtLevel: max） | 规划、架构、疑难调试、核心实现、代码审查 |
| flash | `glm-roles:flash` | GLM-5.3-Flash（thoughtLevel: high） | 信息搜集、批量小修改、简单查询、格式化 |

用法：
- `/glm-roles:deep <任务>` — 手动指派深度角色
- `/glm-roles:flash <任务>` — 手动指派快速角色
- 不输命令时，主 agent 按 glm-roles 技能里的分流规则自动选角色
- `/glm-roles:models` — 列出当前登录可见的全部模型（按套餐分组），给角色挑模型用
- `/glm-roles:assign <角色> <模型ID>` — 把某角色的模型改为任意套餐下的指定模型

说明：主对话模型不受本插件影响，仍由用户在界面里切换；插件只固定子任务角色的模型。模型 ID 绑定当前套餐（account:zai-start-plan），换套餐后需更新 `agents/*.md` 里的 `model:` 字段。
