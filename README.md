# zcode-glm-roles

GLM Roles —— 一个 ZCode 插件市场，目前包含插件 **glm-roles**：把子任务按角色分流到不同的 GLM 模型，深度任务走 GLM-5.3，轻量任务走 GLM-5.3-Flash；角色绑定的模型可跨套餐自由指派。

## 安装

1. 在 ZCode 中打开 **插件市场 → 添加 → 添加插件市场**，填入本仓库（`Assiby11125/zcode-glm-roles`）。
2. 在 **个人 → assiby-plugins** 找到 **GLM Roles**，点击安装。
3. 新建会话即可使用：

```
/glm-roles:models                      查看当前可用的全部模型（按套餐分组）
/glm-roles:assign deep <模型ID>        把深度角色绑到指定模型
/glm-roles:assign flash <模型ID>       把快速角色绑到指定模型
/glm-roles:deep <任务>                 手动派发深度角色
/glm-roles:flash <任务>                手动派发快速角色
/glm-roles:roles                       查看角色与模型映射
```

不输命令时，主 agent 会按内置分流规则自动选角色（简单任务默认 flash）。

## 说明

- 模型 ID 形如 `account:<套餐>/<模型>`，任何当前登录可见套餐的模型都能绑给任何角色；插件不包含任何密钥或账户凭据。
- 主对话模型由用户在 ZCode 界面自行切换，本插件只管理子任务角色的模型。
- 插件源码在 [glm-roles/](./glm-roles/)，清单见 [.claude-plugin/marketplace.json](./.claude-plugin/marketplace.json)。
