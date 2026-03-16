# OpenClaw Security Audit / 安全巡检

<p align="center">
  <strong>🛡️ 一键系统安全扫描，通俗易懂的安全报告</strong><br>
  <strong>One-click System Security Scan with Human-Friendly Reports</strong><br>
  <sub>OpenClaw Skill | Cross-Platform | Automated Daily Audits</sub>
</p>

---

## 🇨🇳 中文指南

### 项目简介

**OpenClaw 安全巡检** 是一款 OpenClaw Skill，一键执行全面的系统安全扫描，覆盖 14 个关键安全维度，并生成通俗易懂的安全报告。

### ✨ 功能特性

- 🔍 **14 项安全检测** — 环境、文件完整性、SSH、网络暴露面、权限提升等
- 🌐 **威胁情报查询** — 联网查询恶意组件库（可选）
- 📊 **通俗易懂** — 大白话解读，✅/⚠️/🚨 一目了然
- ⏰ **定时自动巡检** — 一句话设置每日自动执行
- 🔒 **隐私优先** — 支持纯本地模式
- 🖥️ **跨平台** — macOS、Linux、Windows

### 📋 检测项目

1. 核心运行环境健康度
2. 系统敏感目录防篡改监控
3. 网关进程内存凭证隔离检查
4. 核心配置防篡改与权限基线
5. 组件与插件供应链完整性
6. 远程访问与爆破攻击监控
7. 网络暴露面与异常进程排查
8. 自动化任务与后门驻留排查
9. 高危命令与越权行为审计
10. 异常外联与数据外泄监控
11. 系统凭证与敏感文件访问审计
12. 硬编码密钥与助记词防泄漏扫描
13. 特权提权（Sudo）操作对账审计
14. 生态组件恶意威胁情报扫描

### 🚀 安装（OpenClaw）

**环境要求：**
- Node.js v18 或更高版本
- OpenClaw CLI 已安装

**🌟 最简单的方法 — 直接对话安装（推荐）：**

在 OpenClaw 对话框中直接说：

```
帮我安装安全巡检 skill，地址是 https://gitee.com/ctct-ct2/openclaw-security-watchdog
```

会自动帮你执行命令完成安装。

**本地安装（开发调试）：**

```bash
# 克隆到本地
git clone https://gitee.com/ctct-ct2/openclaw-security-watchdog.git

# 添加本地 Skill
openclaw skill add ./openclaw-security-audit
```

### 🎯 使用

安装完成后，在 OpenClaw 会话中直接说：

```
执行安全巡检
```

**或更简单：**

```
帮我检查系统安全
```

会自动识别并执行安全巡检 Skill。

或：

```
安全检查
security audit
系统安全扫描
```

openclaw 会引导你完成：
1. **选择检测模式** — 完整检测（推荐）或仅本地
2. **查看结果** — 通过/失败统计、安全评分
3. **获取解读** — 用大白话分析每项结果（可选）

### ⏰ 设置定时巡检

**最简单 — 直接说：**

```
设置每天自动巡检
```

自动帮你配置定时任务。



### 🔒 隐私说明

| 模式 | 说明 |
|------|------|
| 完整检测 | 发送：检测项名称、结果摘要、设备标识（哈希）<br>不发送：文件内容、密码、密钥、日志原文 |
| 仅本地 | 零网络流量，所有数据留在本机 |

报告保存在：`~/.openclaw/security-reports/`
