# Phishing Emails in Action

## [Phishing Emails in Action on TryHackMe](https://tryhackme.com/room/phishingemails5fgjlzxc)

## Overview

* **Difficulty:** Easy
* **Category:** Phishing Analysis / Email Forensics / OSINT
* **Skills Practiced:** Email header analysis, SPF and DMARC validation, IP reputation analysis, attachment hashing, malware investigation

## Tools & Techniques

* Mousepad – for viewing the raw email source
* `sha256sum` – for generating file hashes
* `dig` – for querying DNS records
* IPInfo – for IP address ownership lookup
* Dmarcian SPF Survey – for validating SPF records
* Dmarcian Domain Checker – for checking DMARC records
* VirusTotal – for malware and file reputation analysis

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Start the **Phishing Emails in Action** machine on TryHackMe.
* Open the email named **The Greenholt Phish**.

### 2. Inspecting the Email Header

The email header contains useful information about the sender and message routing.

1. Locate the **Subject** field.

**Finding:**

The **Transfer Reference Number** is included in the subject line.

2. Locate the **From** field.

**Finding:**

Identify the sender's name and email address.

3. Locate the **Reply-To** field.

**Finding:**

This field reveals the email address that will receive replies instead of the sender's address.

### 3. Finding the Originating IP Address

Open the email source using **Mousepad**.

Scroll through the headers until you reach the **Content analysis details** section.

**Finding:**

Locate the originating IP address.

Use **IPInfo** to determine the owner of the IP address.

https://ipinfo.io/

### 4. Checking the SPF Record

Identify the domain listed in the **Return-Path** header.

Query its SPF record using `dig`.

```bash
dig txt <return-path-domain>
```

Alternatively, use the **Dmarcian SPF Survey**.

https://dmarcian.com/spf-survey/

Review the returned TXT record to determine the configured SPF policy.

### 5. Checking the DMARC Policy

Use the **Dmarcian Domain Checker** to inspect the domain's DMARC configuration.

https://dmarcian.com/domain-checker/

Review the returned DMARC policy for the sender's domain.

### 6. Investigating the Attachment

Search the email source for:

```
filename
```

The attachment filename will be displayed in the email headers.

Save the attachment to your machine.

Generate its SHA256 hash.

```bash
sha256sum <saved-file-path>
```

### 7. Analyzing the Attachment

Open **VirusTotal**.

https://www.virustotal.com/gui/home/upload

Search using the SHA256 hash generated in the previous step.

**Findings:**

VirusTotal reveals additional information about the attachment, including:

* File size
* Actual file extension
* Detection results
* Additional threat intelligence
