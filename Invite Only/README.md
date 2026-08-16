# Invite Only

## [Invite Only on TryHackMe](https://tryhackme.com/room/invite-only)

## Overview

* **Difficulty:** Easy
* **Category:** Malware Analysis / Threat Intelligence / OSINT
* **Skills Practiced:** Malware hash investigation, process tree analysis, dropped-file investigation, malware family identification, IP reputation analysis, threat intelligence research

## Tools & Techniques

* **TryDetectThis 2.0** – for investigating the flagged malware hash

## Walkthrough (Step-by-Step)

### 1. Investigating the Flagged Hash

1. Open **TryDetectThis 2.0**.

2. Locate the flagged hash provided by the room.

3. Open the hash and inspect the file information.

**Finding:**

The analysis provides the **filename** and **file type** associated with the flagged hash.

### 2. Investigating Execution Parents

1. Open the **Relations** tab.

2. Find the `Execution Parents` section.

Here we can identify the process that executed or launched the suspicious file.

3. Inspect the execution parent information.

The parent process also has an associated hash.

### 3. Investigating Dropped Files

While still in the **Relations** tab, find:

```text
Dropped Files
```

This section shows files created or dropped by the analyzed malware.

**Finding:**

The dropped file can be identified from the listed files.

### 4. Investigating the Parent File

The execution parent identified earlier also has its own hash.

1. Open the parent execution file using its hash.

2. Go to the **Relations** tab.

3. Check:

```text
Dropped Files
```

The dropped files listed here reveal additional malicious files associated with the infection.

**Finding:**

The malicious dropped file can be identified from the parent process's relationships.

### 5. Identifying the Malware Family

The next question requires investigating the flagged IP address.

1. Open the analysis associated with the flagged IP.

2. Go to the **Community** tab.

3. Review the information provided by the security community.

Look for the malware family associated with the IP.

**Finding:**

The malware family can be identified from the community reports.

### 6. Investigating the Flagged IP

Search the flagged IP address using Google:

```text
<flagged-ip>
```

Look through the security and threat intelligence reports associated with the IP.

One of the reports contains information about the attack campaign.

**Finding:**

The title of the relevant report provides the answer to the question.

### 7. Investigating the Attack

Continue reading the threat intelligence report.

The report contains additional information about the attack, including:

* The tool used to steal browser cookies
* The type of phishing attack used
* The platform targeted by the campaign

Look for the sections describing the attack technique and the malware's capabilities.

**Findings:**

The report reveals:

```text
Cookie-stealing tool → <tool identified in report>
Phishing type        → <phishing technique identified in report>
Platform             → <targeted platform identified in report>
```

These details provide the remaining answers for the room.
