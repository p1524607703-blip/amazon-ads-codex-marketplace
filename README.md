# 亚马逊广告数据分析（Codex 运营只读插件）

这是运营团队使用的 Codex 公开安装包。公开仓库只包含插件清单、只读使用说明和应用关联，不包含数据库地址、密码、证书、Tunnel ID、API key、广告原始数据或客户数据。员工电脑不直连阿里云 RDS。

## 发给运营的唯一链接

管理员只需把本 GitHub 仓库链接发给运营。运营先安装并登录公司 Codex/OpenAI 工作区，然后把链接粘贴给 Codex，并发送：

> 安装这个 GitHub 链接里的 `amazon-ads-ops-readonly` 插件。安装完成后新建任务，检查总工具 10、只读工具 10、写入工具 0、管理员工具 0；任一数量不符就停止使用。

Codex 会添加团队 Marketplace、安装插件并打开必要的 OpenAI 应用授权。运营不需要执行命令，也不需要配置数据库。

仓库公开不代表数据库公开。真正的数据访问仍由公司 OpenAI 工作区成员资格、独立只读应用、Secure MCP Tunnel 和 RDS 只读角色共同控制。

## 管理员命令行备用方式

```bash
codex plugin marketplace add p1524607703-blip/amazon-ads-codex-marketplace
codex plugin add amazon-ads-ops-readonly@amazon-ads-team
```

安装后必须新建一个 Codex 任务，再调用 `get_ads_data_context` 验收：

- `registered_tool_count = 10`
- `read_only_tool_count = 10`
- `write_tool_count = 0`
- `database_admin_tool_count = 0`

## 权限边界

- 允许：查询广告表现、成熟归因、TIB、产品分类、入库健康和待复查清单。
- 禁止：报表导入、任何新增/更新/删除、任意 SQL、DDL、数据库角色与权限管理。
- 后端使用独立 `SELECT` 账号并强制只读事务。
- 本插件与负责人现用的 25 工具应用完全独立，不修改也不替代现有应用。

如果工具目录出现 `import_ad_report`、`execute_database_admin_sql`、`create_*`、`update_*`、`delete_*` 或 `upsert_*`，立即停止使用并联系管理员。
