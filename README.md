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

## Keeping the lab files in order

Each lab keeps its own folder with the write-up, the exact queries used, and any screenshots or exports from the run. Name files with the lab name and the date of the run, so a rerun a month later can sit next to the first run without overwriting it. When a query changes between runs, keep both versions and note which run each one belongs to.


## Retesting after a log source changes

When a log source is renamed, a forwarder is moved, or a field format changes, a detection can go quiet without anything being wrong on the hosts:

- Rerun the known test event first and confirm it still appears in the new source with the fields the query expects.
- Compare the hit count for a normal day before and after the change, and note any drop that the change explains.
- Update the write-up with the new source name and the date of the change, so older results stay readable.

## When a rerun gives a different result

If the same query on the same time range returns a different count on a rerun, do not just replace the old result:

- Check for late arriving logs first. A source that was still indexing during the first run can add events afterwards.
- Check that the time range, time zone, and log source are exactly the same as the first run.
- Check whether the query or a field extraction changed between runs, and keep both versions if it did.
- Note both counts and the reason found in the write-up, so the change is explained rather than hidden.

## Separating test events from real activity

Labs rely on known test events, but those events should not be counted as findings:

- Note the test host, user, and time in the write-up before running the search, so the test can be told apart later.
- Leave test events out of the hit counts reported for real activity, and say in the write-up that they were left out.
- If a search returns only the test event, report that plainly. The detection fired, but no real activity was found in that range.

## Starting a search with a narrow window

Before running a query over a full day or week, start with a short window around a known event:

- Pick a window of a few minutes around the test event and confirm the event appears with the fields the query needs.
- If it does not appear, fix the source, time zone, or field problem on the small window first. It is faster to check and costs less search time.
- Only widen the window once the small search is correct, and note the wider range used in the write-up.

---

## 🧰 Tools Used
- Splunk
- Windows Event Logs
- Sysmon
- Kali Linux
