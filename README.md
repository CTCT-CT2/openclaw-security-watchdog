<h1 align="center">WorkBuddy 安全巡检</h1>

<p align="center">
  <strong>🛡️ 一键系统安全扫描，通俗易懂的安全报告</strong><br>
  <strong>One-click System Security Scan with Human-Friendly Reports</strong><br>
  <sub>WorkBuddy Skill | Cross-Platform | Automated Daily Audits</sub>
</p>

---

## 🇨🇳 中文指南

### 项目简介

**WorkBuddy 安全巡检** 是一款 WorkBuddy 安全巡检工具，一键执行系统安全扫描并生成通俗易懂的报告。它覆盖关键安全维度，帮助用户快速了解并排查系统安全状况。

### ✨ 功能特性

- **14 项安全检测** — 环境、文件完整性、SSH、网络暴露面、权限提升等。
- **威胁情报查询** — 可选的完整检测模式，联网查询生态组件恶意威胁情报和同步安全评分。
- **通俗易懂** — 报告解读支持大白话分析，使用 ✅（安全）、⚠️（建议处理）、🚨（严重问题）图标，结果一目了然。
- **定时自动巡检** — 支持通过 WorkBuddy 内置 Automation 设置每天自动执行。
- **隐私优先** — 默认使用本地离线模式，零网络流量，报告仅保存在本机。
- **跨平台** — 兼容 macOS、Linux、Windows，均通过只读系统命令执行查询。

### 🚀 安装 (WorkBuddy)

**环境要求：**
- Node.js v18 或更高版本
- WorkBuddy 客户端已就绪

**🌟 最简单的方法 — 直接对话安装（推荐）：**

在 WorkBuddy 对话框中直接说：

```text
帮我安装安全巡检 skill
```

### 🎯 使用

安装完成后，在 WorkBuddy 会话中直接说：

```text
执行安全巡检
```

**支持的其他触发词：**
用户可以说"安全巡检"、"安全检查"、"安全审计"、"巡检"、"security audit"、"检查安全"、"系统安全"等来触发此技能。

WorkBuddy 会引导你完成：
1. **选择检测模式** — 仅本地扫描（默认，不联网）或完整检测（--push，需用户明确确认同意）。
2. **查看结果** — 提供 PASS/FAIL/SKIP 统计、系统安全得分以及报告路径。
3. **获取解读** — 只有当用户主动回复需要分析时，才会逐项用通俗易懂的语言解读安全报告。

### ⏰ 设置定时巡检

**最简单 — 直接说：**

```text
设置每天自动巡检
```

⚠️ **定时任务安全要求：**
- **必须**使用 WorkBuddy 内置的 Automation 机制进行配置，**禁止**使用系统 crontab。
- **严禁**在 Automation 定时任务中使用 `--push` 参数。定时任务必须仅在本地离线模式下运行，防止设备信息被长期自动上报。

### 🔒 隐私说明

本技能有两种运行模式，隐私风险等级不同：

| 模式 | 行为说明 | 数据上报情况 |
|------|------|------|
| **模式 1：本地离线（默认）** | 适合离线环境或隐私敏感场景。 | **零网络请求，零数据上报**。所有数据和报告均留在本机。 |
| **模式 2：完整检测（--push）** | 联网查询威胁情报并同步安全评分。仅在单次手动运行时可用。 | **需明确确认。** 会向 Changeway 服务器上报：MAC 地址、主机名、持久化 agent_id、本机完整 Skill 清单、每项安全检查的名称和结果摘要。**绝不上传详细命令输出和敏感日志**。 |

**本地文件存储位置：**
- **扫描报告：** `~/.workbuddy-security/security-reports/`
- **持久化 agent_id：** `~/.workbuddy-security/.agent-id`
- **首次运行标记：** `~/.workbuddy-security/.audit-first-run`
- **Skill 哈希基线：** `~/.workbuddy-security/skill-hashes/`

## License

Documentation in this repository is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0).

To view a copy of this license, visit:
https://creativecommons.org/licenses/by-nc-sa/4.0/
