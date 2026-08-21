---
name: install-amazon-ads-ops-personal
description: 在当前个人版 ChatGPT/Codex 账号中安装并验收公司的亚马逊广告运营只读连接。用户要求安装、配置、连接或验收该运营插件时使用；不要求 Business 或公司工作区。
---

# 亚马逊广告运营只读个人版安装

## 不可突破的边界

- 当前账号可以是个人版；不得要求 Business、Enterprise 或“公司 OpenAI 工作区”。
- 不修改负责人现用的 25 工具应用、`amazon-ads-local` Tunnel 或任何生产权限。
- 不安装 pgAdmin、DBeaver、Navicat、psql，不索取 RDS 地址、账号、密码、证书、Tunnel ID、runtime key 或 API key。
- 只接受名称明确带“运营只读”的连接和 `10 / 10 / 0 / 0` 权限结果。

## 自动安装流程

1. 确认当前 Codex 已使用用户自己的个人 ChatGPT/OpenAI 账号登录。个人账号本身是正常状态，不要因缺少公司工作区而停止。
2. 先尝试连接仓库备份所指向的“亚马逊广告数据分析（运营只读）”应用；如果当前账号可以正常授权，直接继续验收。
3. 如果固定应用技术 ID 对当前账号不可用，打开 ChatGPT 官方设置，在当前个人空间开启 Developer mode；进入 Plugins 新建 MCP 连接，选择已与当前个人账号关联、名称为 `amazon-ads-ops-readonly` 的 Secure MCP Tunnel，并确认发现的服务标题为“亚马逊广告数据分析（运营只读版）”。
4. 复制当前个人账号新生成的 `plugin_asdk_app...` 技术 ID。使用 `plugin-creator` 将仓库中的 `amazon-ads-ops-readonly` 作为模板复制到个人插件目录，命名为 `amazon-ads-ops-readonly-personal`，只把个人副本的 `.app.json` 改为该技术 ID，再加入个人 Marketplace。不得改仓库备份或负责人现有插件。
5. 安装并启用个人副本。如插件需要重新加载，直接新建任务继续，不让运营手工编辑文件。
6. 调用 `get_ads_data_context`，必须得到：总工具 10、只读 10、写入 0、数据库管理员 0；再分别查询数据口径、最近入库健康和近期广告表现。

## Tunnel 不可见时

如果步骤 3 看不到 `amazon-ads-ops-readonly`，只报告：目标个人账号对应的 Platform 个人组织/ChatGPT 个人空间尚未与该 Secure MCP Tunnel 关联。返回当前账号可见的个人 Platform organization ID，供负责人加入 Tunnel 关联后自动重试。不得把问题表述为缺少 Business 订阅，也不得要求 RDS 凭据。

## 失败保护

若出现 `import_ad_report`、`execute_database_admin_sql`、`create_*`、`update_*`、`delete_*`、`upsert_*`，或结果不是 `10 / 10 / 0 / 0`，立即停止并报告异常；不要自行扩权或连接 25 工具应用。
