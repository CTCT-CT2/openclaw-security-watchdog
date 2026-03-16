# OpenClaw 安全巡检定时任务配置指南

## ⚠️ 重要警告（必读）

### 必须使用 `openclaw cron`，禁止使用系统 crontab

❌ **错误做法**：使用 `crontab -e` 或编辑 `/etc/crontab`


✅ **正确做法**：使用 OpenClaw 内置的 cron 系统
```bash
openclaw cron add ...
```

**原因**：
1. 系统 crontab 无法正确初始化 OpenClaw 环境变量和会话
2. 会导致执行失败、权限问题或推送异常
3. `openclaw cron` 自动处理隔离会话、超时、推送等逻辑

## 快速配置

### 使用 OpenClaw Cron 注册

```bash
openclaw cron add \
  --name "changeway-security-audit" \
  --description "每晚安全巡检" \
  --cron "45 23 * * *" \
  --tz "Asia/Shanghai" \
  --session "isolated" \
  --message "Execute this command and output the result as-is, no extra commentary: node <skill-path>/scripts/openclaw-hybrid-audit-changeway.js --push" \
  --announce \
  --channel <channel> \
  --to <your-chat-id> \
  --timeout-seconds 900 \
  --thinking off
```

### 参数说明

| 参数 | 说明 | 建议值 |
|------|------|--------|
| `--name` | 任务唯一标识 | `changeway-security-audit` |
| `--cron` | Cron 表达式 | `23 45 * * *` (每天 23:45) |
| `--tz` | 时区 | `Asia/Shanghai` 或你的本地时区 |
| `--session` | 会话类型 | `isolated` (隔离环境更安全) |
| `--message` | 执行命令 | 见下方注意事项 |
| `--timeout-seconds` | 超时时间 | `300` (必须 ≥300s，否则可能超时失败) |

## 注意事项

### 关于 --push 参数

**推荐带 `--push`**，原因：
1. 威胁情报每日更新，定时任务开启可及时发现新风险
2. 云端基线对比能发现本地难以察觉的异常
3. 仅上报组件清单，无敏感信息泄露风险


### ⚠️ 避坑指南

1. **timeout 必须 ≥ 300s**
   - isolated session 需要冷启动 Agent
   - 120s 会因超时被杀

2. **--to 必须用 chatId**
   - 不能用用户名（如 "L"）
   - Telegram 需要数字 chatId

3. **message 不要写"发送给某人"**
   - isolated Agent 无法解析用户名
   - 推送由 `--announce` 框架处理
   - 维持示例写法，但需关注`--push`参数设置

4. **推送偶发失败**
   - Telegram 等平台偶发 502/503
   - 报告始终保存在本地：`~/.openclaw/security-reports/`
   - 查看历史：`openclaw cron runs --id <jobId>`

## 查看巡检历史

```bash
# 列出所有 cron 任务
openclaw cron list

# 查看特定任务的执行历史
openclaw cron runs --id <job-id>
```

## 修改/删除定时任务

```bash
# 删除任务
openclaw cron remove --id <job-id>

# 重新添加（先删后加）
openclaw cron remove --id nightly-security-audit
openclaw cron add ...
```

## 手动测试

配置前先手动测试脚本能否正常执行：

```bash
# 进入 skill 目录
cd <skill-path>

# 本地模式（无网络）
node scripts/openclaw-hybrid-audit-changeway.js

# 完整模式（含威胁情报，推荐）
node scripts/openclaw-hybrid-audit-changeway.js --push
```

## 常见错误及修复

### 错误 1：使用了系统 crontab
**现象**：任务显示在 `crontab -l` 中，但执行失败或没有推送
**修复**：
```bash
# 1. 删除系统 crontab 中的任务
crontab -e  # 删除相关行

# 2. 使用 openclaw cron 重新添加
openclaw cron add \
  --name "changeway-security-audit" \
  --description "每晚安全巡检" \
  --cron "45 23 * * *" \
  --tz "Asia/Shanghai" \
  --session "isolated" \
  --message "Execute this command and output the result as-is, no extra commentary: node <skill-path>/scripts/openclaw-hybrid-audit-changeway.js --push" \
  --announce \
  --channel <channel> \
  --to <your-chat-id> \
  --timeout-seconds 900 \
  --thinking off
```

### 错误 2：--message 中使用了相对路径
**现象**：定时任务执行时提示找不到脚本
**修复**：使用绝对路径 `<skill-path>/scripts/openclaw-hybrid-audit-changeway.js`

### 错误 3：--session 未设置为 isolated
**现象**：任务执行时权限错误，或无法访问安全报告
**修复**：必须添加 `--session "isolated"`

### 错误 4：timeout 设置过短
**现象**：任务被强制终止，报告不完整
**修复**：`--timeout-seconds 600`（必须 ≥ 600）
