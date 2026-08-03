# Snapped Phishing Line

## [Snapped Phishing Line on TryHackMe](https://tryhackme.com/room/snappedphishingline)

## Overview

* **Difficulty:** Easy
* **Category:** Phishing Analysis / Digital Forensics / OSINT
* **Skills Practiced:** Email analysis, phishing investigation, Base64 decoding, URL analysis, malware archive analysis, VirusTotal investigation, IOC extraction

## Tools & Techniques

* `grep` – for searching phishing emails and extracting indicators
* Email Source Analysis – for inspecting raw email content and attachments
* `CyberChef` – for Base64 decoding and URL defanging
* `sha256sum` – for generating file hashes
* `VirusTotal` – for malware, domain, and certificate investigation
* `grep regex` – for extracting email addresses from phishing kit files

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Start the **Snapped Phishing Line** machine on TryHackMe.
* Access the provided phishing email files.

### 2. Identifying the Phishing Email

The first step is to find which email contains a PDF attachment. Use `grep` to search through the phishing emails:

```bash
grep -i pdf <path/to/phish/emails>
```

**Findings:**

The command reveals the email file containing a PDF attachment. Open the identified email and check the recipient. The phishing email was sent to:

```
Zoe Duncan
```

### 3. Extracting the Malicious Attachment

1. Open the email sent to **Zoe Duncan**. View the **Message Source** and inspect the raw email contents. Inside the source, locate the attachment. The attachment is encoded using **Base64**. Copy the encoded data and decode it using [CyberChef](https://gchq.github.io/CyberChef).

After decoding:

* Extract the URL contained inside the attachment.
* Defang the URL before analysis.

### 4. Analyzing the Phishing URL

1. Enumerate the discovered phishing URL. During investigation, a ZIP archive hosted on the phishing infrastructure can be found. Download the archive for further analysis. Generate the SHA256 hash of the downloaded file:

```bash
sha256sum <archive.zip>
```

**Output:**

```
<SHA256 hash>
```

### 5. Malware Analysis with VirusTotal

1. Submit the generated hash to [VirusTotal](https://www.virustotal.com/gui/home/upload).

2. Analyze the results to gather information about the phishing archive.

3. Next, investigate the phishing domain using VirusTotal.

4. Navigate to:

```
Relations
	└── Historical SSL Certificates
```

5. Find when the SSL certificate used by the phishing domain to host the phishing kit archive was first logged.

**Finding:**

```
<certificate date>
```

### 6. Investigating the Phishing Server

1. Navigate to the discovered phishing domain:

```
<found-domain>/data/Update365
```

2. Inside the directory, locate:

```
log.txt
```

3. Review the log file.

The logs reveal which user submitted their password twice.

**Finding:**`
<found-domain>/data/Update365/office365/flag.txt
``

```
<username>
```

### 7. Analyzing the Phishing Kit

1. Extract the downloaded ZIP archive. Search through the extracted files to identify email addresses used by the attacker. Use:

```bash
grep -r -o '[A-Za-z0-9._%+-]\+@[A-Za-z0-9.-]\+\.[A-Za-z]\{2,6\}' .
```

**Findings:**

The command reveals the email address used by the adversary to send phishing emails.

```
<attacker-email>
```

### 8. Retrieving the Flag

1. Navigate to:

```
<found-domain>/data/Update365/office365/flag.txt
```

2. The file contains an encoded flag. Decode the value to obtain the final answer.
