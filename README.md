# 🛡️ SOC & Blue Team Labs

This repository contains hands-on SOC analyst labs focused on:
- Log analysis
- Threat detection
- Incident response
- Splunk SPL queries

All labs are based on real-world SOC scenarios.

---

## 📂 Lab Categories

### 🔍 Splunk Detections
- [DoS detection using firewall logs](splunk/DoS%20Detection%20Using%20Firewall.md)
- Brute force login detection
- Suspicious PowerShell activity
- Persistence techniques

### 🚨 Incident Response
- Ransomware investigation
- Malware execution analysis

### 🕵️ Threat Hunting
- Registry-based persistence
- Abnormal process behavior

---

## How to read a lab

Each completed lab write-up follows the same order:

- What the lab set out to check, in plain terms
- The lab layout and tools used
- Problems hit during setup, and the fix that worked
- The Splunk queries used, with the log source they ran against
- Screenshots or results from the run
- What was learned, including what did not work at first

That order is deliberate. In SOC work the setup and troubleshooting are part of the result, not extra detail.

---

## 🧰 Tools Used
- Splunk
- Windows Event Logs
- Sysmon
- Kali Linux
