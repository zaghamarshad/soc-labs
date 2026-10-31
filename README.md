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

## Writing up a new lab

When a new lab is added, keep the same order:

- State the goal in one or two plain sentences before any query.
- Name the log source and the time range the search covered.
- Paste the exact SPL used, not a cleaned up version, so the result can be repeated.
- Note any setup problem and the fix, even when the fix was simple.
- End with what the result showed and what would be checked next.

## Checking a detection before trusting it

A query that returns results is not yet a detection worth relying on. Before a lab result is treated as real, check it in this order:

- Run the same query on a time range with no known test event and note what it returns. Hits there are false positives to explain, not ignore.
- Confirm the log source actually covers the host or user in the event. A silent gap in logging can look like a clean result.
- Check the time zone and time range on both the log source and the search, so an event is not missed by an hour offset.
- Keep one known test event for each detection, so the query can be rerun after a change to confirm it still fires.

---

## 🧰 Tools Used
- Splunk
- Windows Event Logs
- Sysmon
- Kali Linux
