# WorkBuddy 安全巡检定时任务配置指南

## ⚠️ 重要警告（必读）

### 必须使用 WorkBuddy Automation，禁止使用系统 crontab

❌ **错误做法**：使用 `crontab -e` 或编辑 `/etc/crontab`

✅ **正确做法**：使用 WorkBuddy 内置的 Automation 机制（通过 automation_update 工具创建）

**原因**：
1. 系统 crontab 无法正确初始化 WorkBuddy 环境变量和工作区
2. WorkBuddy Automation 自动处理工作区路径、调度和超时逻辑
3. Automation 与 WorkBuddy 深度集成，可在 IDE 内直接管理

## 快速配置

### 使用 WorkBuddy Automation 注册定时巡检

调用 `automation_update` 工具，参数如下：

```
mode: "suggested create"
name: "security-audit-daily"
prompt: "执行安全巡检脚本：node <SKILL_DIR>/scripts/openclaw-hybrid-audit-changeway.js — 然后从输出中提取并只汇报以下三项：(1) 包含 PASS/FAIL/SKIP 数量的那一行，(2) 以「详细审计报告已保存至」开头的报告文件路径。不要输出脚本完整原始内容。"
scheduleType: "recurring"
rrule: "FREQ=DAILY;BYHOUR=23;BYMINUTE=45"
cwds: "<当前工作区路径>"
status: "ACTIVE"
```

其中 `<SKILL_DIR>` 需替换为本 SKILL.md 所在目录的绝对路径。

> ❌ **禁止在 Automation 的 `prompt` 中添加 `--push`**。`--push` 会向远端持续发送设备标识（MAC、主机名、agent_id）和 Skill 清单，不适合在未经每次人工确认的定时任务中使用。如需使用完整检测，请手动运行并在 SKILL.md 第三步选择并确认。

### 参数说明

| 参数 | 说明 | 建议值 |
|------|------|--------|
| `name` | 任务唯一标识 | `security-audit-daily` |
| `rrule` | 调度规则（iCalendar RRULE 格式） | `FREQ=DAILY;BYHOUR=23;BYMINUTE=45`（每天 23:45） |
| `scheduleType` | 调度类型 | `recurring`（持续重复） |
| `cwds` | 工作区目录 | 当前 WorkBuddy 工作区路径 |
| `prompt` | 执行指令 | 见上方示例，绝对路径引用脚本 |
| `status` | 任务状态 | `ACTIVE`（立即激活） |

## 注意事项

### 关于 --push 参数

❌ **Automation 定时任务中严禁使用 `--push`**。

`--push` 会向远端持续发送设备标识（MAC 地址、主机名、持久化 agent_id）和本机完整 Skill 清单，属于隐私敏感操作，必须每次由用户手动确认后才能执行。将 `--push` 写入 Automation 会导致设备信息被长期自动上报，违背知情同意原则。

如需使用完整检测（威胁情报查询），请**手动运行**安全巡检并在第三步选择并完成知情确认。

### ⚠️ 避坑指南

1. **prompt 中必须使用脚本绝对路径**
   - 不要使用相对路径，Automation 运行时工作目录不确定
   - 正确示例：`node /Users/yourname/.workbuddy/skills/ctct-security-patrol/scripts/openclaw-hybrid-audit-changeway.js`

2. **rrule 时间格式**
   - WorkBuddy Automation 使用 iCalendar RRULE 格式
   - 每天 23:45 对应：`FREQ=DAILY;BYHOUR=23;BYMINUTE=45`
   - 每周一 09:00 对应：`FREQ=WEEKLY;BYDAY=MO;BYHOUR=9;BYMINUTE=0`

3. **报告始终保存在本地**
   - 报告路径：`~/.workbuddy-security/security-reports/`
   - 即使 Automation 执行完成后，随时可以查阅历史报告

## 查看与管理定时任务

在 WorkBuddy 中，可以通过以下方式管理 Automation：

- **查看已有任务**：调用 `automation_update` 工具，`mode: "view"`，并提供对应的 `id`
- **暂停任务**：调用 `automation_update` 工具，`mode: "suggested update"`，将 `status` 改为 `PAUSED`
- **修改调度时间**：调用 `automation_update` 工具，`mode: "suggested update"`，更新 `rrule` 字段
- **删除任务**：在 WorkBuddy IDE 的自动化管理界面中删除对应任务

## 手动测试

配置前先手动测试脚本能否正常执行：

```bash
# 进入 skill 目录
cd <skill-path>

# 本地模式（无网络）
node scripts/openclaw-hybrid-audit-changeway.js

# 完整模式（含威胁情报）— ⚠️ 仅限手动运行，不得写入 Automation
# 运行前须在 SKILL.md 第三步完成知情确认（回复"2 已了解"）
node scripts/openclaw-hybrid-audit-changeway.js --push
```

## 一次性定时提醒（非每日重复）

如果用户只想在某个特定时间运行一次巡检，使用 `scheduleType: "once"`：

```
mode: "suggested create"
name: "security-audit-once"
prompt: "执行安全巡检脚本：node <SKILL_DIR>/scripts/openclaw-hybrid-audit-changeway.js — 从输出中提取并汇报 PASS/FAIL/SKIP 统计和报告保存路径。"
scheduleType: "once"
scheduledAt: "2026-04-03T23:45"
cwds: "<当前工作区路径>"
status: "ACTIVE"
```

## 常见问题

### 问题 1：不知道 SKILL_DIR 路径
**解决**：Skill 安装在 `~/.workbuddy/skills/ctct-security-patrol/`（用户级）或 `{workspace}/.workbuddy/skills/ctct-security-patrol/`（项目级）。可通过 SKILL.md 所在目录确认。

### 问题 2：Automation 未在预定时间执行
**解决**：
- 确认任务 `status` 为 `ACTIVE`
- 确认 WorkBuddy IDE 在计划执行时间处于运行状态
- 使用 `automation_update` 工具查看任务详情确认配置

### 问题 3：报告文件找不到
**解决**：报告默认保存在 `~/.workbuddy-security/security-reports/report-YYYY-MM-DD.txt`。确认脚本执行成功（无错误输出）后再查找。
