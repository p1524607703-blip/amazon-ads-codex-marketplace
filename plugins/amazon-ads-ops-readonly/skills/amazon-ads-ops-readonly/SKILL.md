---
name: amazon-ads-ops-readonly
description: 使用公司只读 Amazon Ads 应用查询广告活动表现、成熟归因、TIB、产品分类、入库健康和待复查清单。运营人员提出亚马逊广告分析、诊断、复盘或优化建议时使用。严禁写入、导入、删除或执行 SQL。
---

# 亚马逊广告运营只读分析

## 权限红线

- 只能调用应用提供的 10 个只读查询工具。
- 不得导入文件，不得新增、更新或删除任何数据库记录。
- 不得执行任意 SQL、DDL、角色管理或权限变更。
- 如果用户要求修改数据库或开放权限，明确说明此插件不具备该能力，并将建议以文本形式返回，等待有权限的负责人另行执行。
- 不得向用户显示数据库主机、IP、端口、账号、密码、证书、隧道 ID 或应用技术 ID。

## 分析流程

1. 先调用 `get_ads_data_context`，确认当前目录为 10 个只读工具、写入工具为 0、管理员工具为 0。
2. 近期流量和花费观察使用 `get_recent_campaign_performance`。
3. 最终 ROAS、销售额和转化结论优先使用 `get_mature_campaign_performance`。
4. 单活动趋势使用 `get_campaign_daily_history`；预算内活跃时间使用 `get_latest_tib`。
5. 需要跟进时读取 `get_campaigns_due_for_review` 或 `list_managed_campaigns`，只输出建议清单，不写回数据库。
6. 报告数据可能过期时读取 `get_import_health` 和 `get_attribution_refresh_due`，提示负责人重新导出或入库，不自行导入。
7. 结论必须标明日期范围、归因成熟度和关键样本限制。

## 失败保护

- 如果工具目录出现 `import_ad_report`、`execute_database_admin_sql`、`create_*`、`update_*`、`delete_*` 或 `upsert_*`，立即停止使用该连接并提示管理员检查插件版本。
- 如果 `get_ads_data_context` 返回写入工具数量不为 0 或数据库管理员工具数量不为 0，立即停止分析。
- 如果应用不可用，只报告连接问题；不要要求运营人员配置 RDS 白名单、安装数据库客户端或索取数据库密码。
