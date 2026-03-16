<h1 align="center">OpenClaw Security Watchdog</h1>

<p align="center">
<strong>🛡️ One-click System Security Scan with Human-Friendly Reports</strong>




<sub>OpenClaw Skill | Cross-Platform | Automated Daily Audits</sub>
</p>

---

## 🇬🇧 English Guide

### Project Introduction

**OpenClaw Security Watchdog** is an OpenClaw Skill that performs a comprehensive, one-click system security scan covering 14 key security dimensions and generates easy-to-understand security reports.

### ✨ Features

* 🔍 **14 Security Checks** — Environment, file integrity, SSH, network attack surface, privilege escalation, etc.
* 🌐 **Threat Intelligence Lookup** — Online queries of malicious component databases (optional).
* 📊 **Human-Friendly** — Plain English explanations, with ✅/⚠️/🚨 status clear at a glance.
* ⏰ **Scheduled Automated Audits** — Set up daily automated executions with a single sentence.
* 🔒 **Privacy First** — Supports a pure local mode.
* 🖥️ **Cross-Platform** — macOS, Linux, and Windows.

### 📋 Inspection Items

1. Core Runtime Environment Health
2. System Sensitive Directory Tamper Monitoring
3. Gateway Process Memory Credential Isolation Check
4. Core Configuration Anti-Tampering & Permission Baselines
5. Component & Plugin Supply Chain Integrity
6. Remote Access & Brute-Force Attack Monitoring
7. Network Attack Surface & Abnormal Process Investigation
8. Automated Tasks & Backdoor Persistence Investigation
9. High-Risk Command & Unauthorized Behavior Audit
10. Abnormal Outbound Connections & Data Leakage Monitoring
11. System Credential & Sensitive File Access Audit
12. Hardcoded Key & Mnemonic Leakage Scan
13. Privilege Escalation (Sudo) Operation Reconciliation Audit
14. Ecosystem Component Malicious Threat Intelligence Scan

### 🚀 Installation (OpenClaw)

**Requirements:**

* Node.js v18 or higher
* OpenClaw CLI installed

**🌟 Easiest Method — Conversational Installation (Recommended):**

Simply say this in the OpenClaw chat:

```text
Help me install the security watchdog skill from https://github.com/CTCT-CT2/openclaw-security-watchdog

```

It will automatically execute the command to complete the installation for you.

**Local Installation (Development & Debugging):**

```bash
# Clone locally
git clone https://github.com/CTCT-CT2/openclaw-security-watchdog.git

```

### 🎯 Usage

After installation, simply say in the OpenClaw session:

```text
Execute security watchdog

```

**Or even simpler:**

```text
Help me check system security

```

It will automatically recognize and execute the Security Watchdog Skill.


OpenClaw will guide you through:

1. **Selecting a detection mode** — Full detection (recommended) or Local only.
2. **Viewing results** — Pass/Fail statistics and a Security Score.
3. **Getting an interpretation** — Plain English analysis of each result (optional).

### ⏰ Set Scheduled Audits

**The easiest way — Simply say:**

```text
Set up daily automated audits

```

It will automatically configure the scheduled tasks for you.

### 🔒 Privacy Policy

| Mode | Description |
| --- | --- |
| **Full Detection** | **Sends:** Check item name, result summary, device identifier (hash)<br>

<br>**Does NOT send:** File contents, passwords, keys, raw logs |
| **Local Only** | Zero network traffic; all data stays on the local machine |

Reports are saved at: `~/.openclaw/security-reports/`
