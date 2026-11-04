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

## Tuning a noisy detection

When a detection fires too often to review, tune it before turning it down or off:

- Count the hits by host and by user first. Noise often comes from one service account, one scanner, or one admin tool, not from the rule itself.
- Read a few of the noisy events in full and write down what makes them normal, such as the parent process, the source address, or the time of day.
- Exclude only that narrow pattern, then rerun the known test event to confirm the detection still fires on real activity.
- Note the exclusion and the reason in the lab write-up, so a later reader knows the gap was a choice, not an oversight.

## Handing a lab finding to the next shift

When a lab result is passed on, the next analyst should not have to rerun the search to understand it:

- Give the exact query, the log source, and the time range that was searched.
- List the hosts and users in the result, and say what was checked and ruled out.
- Say what should be checked next and by when, in plain terms.
- Link the write-up and any saved screenshots, so the record stands on its own.


## Recording what a lab did not cover

A clean result only covers the logs that were actually searched. At the end of a write-up, note the gaps as plainly as the result:

- Name any log source that was missing, not enabled, or empty for the time range searched.
- Name any host, user group, or event type the lab did not test.
- Say which check would close the gap, such as turning on the missing log source and rerunning the same query.

Writing the gap down stops a later reader from treating a partial search as full coverage.

---

## 🧰 Tools Used
- Splunk
- Windows Event Logs
- Sysmon
- Kali Linux
