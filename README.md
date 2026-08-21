# 亚马逊广告数据分析（Codex 运营只读插件）

这是运营团队使用的 Codex 公开安装包。公开仓库只包含插件清单、只读使用说明和应用关联，不包含数据库地址、密码、证书、Tunnel ID、API key、广告原始数据或客户数据。员工电脑不直连阿里云 RDS。

## 发给运营的唯一链接

运营只需把下面这一句话复制到 Codex：

> 请直接为我安装并验收公司的亚马逊广告运营只读插件：https://github.com/p1524607703-blip/amazon-ads-codex-marketplace

其余安装和安全规则由 Codex 按本仓库自动执行。普通步骤不要反复询问运营；只有 OpenAI 平台强制要求账号本人完成授权时，才提示运营点击确认。

## Codex 自动执行规则

1. 添加或刷新本仓库对应的 `amazon-ads-team` Marketplace，安装并启用 `amazon-ads-ops-readonly`。
2. 如果当前任务不能立即读取新插件，直接新建任务继续验收。
3. 只连接名称明确带“运营只读”的公司应用，不修改或连接负责人现用的 25 工具应用。
4. 调用 `get_ads_data_context`，只接受总工具 10、只读 10、写入 0、管理员 0。
5. 验收后测试数据口径、入库健康和近期广告表现；任一权限数量不符立即停止。

## 运营电脑不需要的配置

- 不需要安装 pgAdmin、DBeaver、Navicat、psql 或其他 PostgreSQL 客户端。
- 不需要提供 RDS 地址、白名单、账号、密码、证书、Tunnel ID 或 API key。
- 不需要让运营电脑直连阿里云 RDS；所有查询通过公司只读应用、Secure MCP Tunnel 和后端只读服务账号完成。

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
