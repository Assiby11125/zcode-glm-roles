# GLM Roles（glm-roles）

仿照 oh-my-opencode 的 roles 思路做的 ZCode 插件：把子任务按角色分流到两个 GLM 模型——**深度任务走 GLM-5.3，轻量任务走 GLM-5.3-Flash**。主对话模型不受影响，仍由你在界面里切换。

## 角色

| 角色 | 子代理类型 | 模型 | 适用任务 |
|------|-----------|------|----------|
| deep | `glm-roles:deep` | `account:zai-start-plan/GLM-5.3`（thoughtLevel: max） | 规划、架构、疑难调试、核心实现、代码审查 |
| flash | `glm-roles:flash` | `account:zai-start-plan/GLM-5.3-Flash`（thoughtLevel: high） | 信息搜集、批量小修改、简单查询、格式化 |

## 用法

- 手动指派：`/glm-roles:deep <任务>`、`/glm-roles:flash <任务>`
- 查看角色：`/glm-roles:roles`
- 自动分流：装好后，主 agent 在派发子任务时会按 `skills/glm-roles/SKILL.md` 里的规则自动选 deep 或 flash（简单任务默认 flash）
- 查可选模型：`/glm-roles:models` — 列出当前登录可见的全部模型，按套餐分组
- 改角色模型：`/glm-roles:assign <角色> <模型ID>` — 见下节「跨套餐指派」

## 跨套餐指派

模型的完整 ID 形如 `account:<套餐>/<模型>`（例如 `account:zai-start-plan/GLM-5.3`）。**任何当前登录可见套餐的模型都可以填进任何角色**，比如 a 套餐的一个模型给 deep、b 套餐的一个模型给 flash：

```
/glm-roles:models
/glm-roles:assign deep account:<套餐a>/<模型X>
/glm-roles:assign flash account:<套餐b>/<模型Y>
```

`/glm-roles:assign` 会改插件源码里对应 agent 的 `model:` 行并同步递增版本号；源码不是热加载，之后需要在界面走一次「插件市场 → 市场源（齿轮）→ dev-assiby-plugins → 刷新该市场 → 个人 → glm-roles → 更新」，新会话生效。

限制：

- 套餐必须出现在当前登录的可用模型列表里（用 `/glm-roles:models` 核对）。两个套餐分属两个账号（两次登录）时，一次会话只能用其中一个账号的模型，这是应用层限制，插件无法绕过。
- 派发时如果某角色绑定的模型不可用，主 agent 会按 SKILL.md 的规则回退默认子代理并提示重新指派。

添加新角色：复制 `agents/deep.md` 为 `agents/<新角色名>.md`，改 frontmatter 里的 `name` 和 `model`（按需改 description/thoughtLevel），升版本号并更新即可，命令和分流规则同理可扩展。

## 目录结构

```
glm-roles/
  .zcode-plugin/plugin.json   插件清单
  agents/deep.md              深度角色（默认 GLM-5.3，model 可指派）
  agents/flash.md             快速角色（默认 GLM-5.3-Flash，model 可指派）
  commands/                   /glm-roles:deep、:flash、:roles、:models、:assign
  skills/glm-roles/SKILL.md   自动分流规则 + 多套餐指派规则
```

## 注意

- `agents/*.md` 里的 `model:` 绑定的是当前套餐的模型 ID（`account:zai-start-plan/...`）。如果以后换了套餐，用新环境里实际可用的模型 ID 更新这两个字段（可在会话里问一下当前可用的模型列表确认）。
- 本地测试市场文件在插件目录旁：`../marketplace.json`，市场名为 `dev-assiby-plugins`；以后改插件请在源码目录里改并递增 `plugin.json` 的版本号，然后在该市场的安装条目上刷新/更新。
