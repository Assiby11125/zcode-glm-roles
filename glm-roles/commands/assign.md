---
description: 把某个角色的模型改为指定套餐下的指定模型（改源码并升版本，需在界面点更新）
argument-hint: "[角色名 deep|flash] [模型ID，形如 account:套餐/模型]"
---

按下面的步骤处理请求：$ARGUMENTS

1. **解析参数**：第一个词是角色名（目前支持 `deep`、`flash`；用户自建角色见 README「添加新角色」），其余部分拼成模型 ID。模型 ID 必须形如 `account:<套餐>/<模型>`；不确定就先跑 `/glm-roles:models` 核对，ID 不在可用列表里时停下来向用户确认，不要盲改。
2. **改角色文件**：编辑插件源码 `agents/<角色名>.md`（优先用当前工作区下的 `plugins/glm-roles/agents/`；不在本工作区时用绝对路径 `C:\Users\Assiby\.zcode\workspace\default\plugins\glm-roles\agents\`），只改 frontmatter 里的 `model:` 一行，其他内容不动。
3. **同步版本号**：把 `.zcode-plugin/plugin.json` 的 `version` 与 `plugins/marketplace.json` 中 glm-roles 条目的 `version` 同步在 patch 位 +1（如 0.2.0 → 0.2.1）。
4. **交付提醒**：用中文告知用户——源码改动不是热加载，需要走「插件市场 → 市场源（齿轮）→ dev-assiby-plugins → 刷新该市场 → 个人 → glm-roles → 更新」，更新后新会话生效。
