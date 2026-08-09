# Have a Break

## [Have a Break on TryHackMe](https://tryhackme.com/room/haveabreak)

## Overview

* **Difficulty:** Medium
* **Category:** Digital Forensics / OSINT / Insider Threat
* **Skills Practiced:** Email header analysis, IP investigation, OSINT, image analysis, log analysis, timeline correlation, employee identification, identity investigation

## Tools & Techniques

* `grep` – for searching through investigation files
* `whois` – for investigating IP ownership
* IPInfo – for identifying the organization associated with an IP address
* Google Maps – for identifying the petrol station from the image clues
* CSV analysis – for investigating employee activity
* Epieos – for investigating the identity associated with an email address
* Email header analysis – for identifying the originating IP address
* Timeline analysis – for correlating suspicious activity with employee activity

## Walkthrough (Step-by-Step)

### 1. Download

Download and extract the investigation files.

The investigation provides several pieces of evidence:

```text
exhibit_a.eml
ecta_memo.html.pdf
exhibit_b.png
access_log.csv
employees.csv
comms_export.txt
```

The investigation concerns a missing shipment of more than 400,000 KitKat products that disappeared while being transported between Italy and Poland.

### 2. Identifying the VPN Used by the Anonymous Sender

The first clue is the anonymous email stored in `exhibit_a.eml`.

1. Open the `.eml` file with a text editor and inspect the email headers.

2. Look for the `Received:` header containing the sender's IP address.

**Finding:**

```text
193.32.249.132
```

3. Investigate the IP address using [**IP Lookup**](https://www.iplocation.net/ip-lookup) service.

### 3. Finding the Petrol Station

The next clue is the dashcam image `exhibit_b.png`.

1. Open the image and inspect the visible information.

The image shows:

* An **ORLEN** petrol station.
* A road sign pointing toward **Olomouc**.
* A road sign pointing toward **Brno**.
* The **D1** highway.
* The investigation materials also indicate that the vehicle was stopped near **Hulín**.

2. Use Google Maps to search for ORLEN petrol stations around Hulín.

The dashcam timestamp places the vehicle at this location on **26 March 2026 at 22:31**.

### 4. Finding the Suspicious Action

The next step is to investigate `access_log.csv`.

The log records employee activity involving files in the route planning system.

1. Open `access_log.csv`.

2. Filter the entries for:

```text
2026-03-25
```

3. Look for unusual actions.

Most entries are normal file operations, but one `EXPORT` operation stands out.

### 5. Identifying the Anonymous Email Sender

The anonymous sender claimed to have witnessed the suspicious activity.

Use the access log to investigate who was working around the time of the suspicious export.

The employee responsible for the export was:

```text
BR-0291
```

Another employee was active later that night:

```text
2026-03-25, 23:41:17, BR-0312, DRIVER_SCHEDULE_WK13.xlsx, EDIT
```

This employee was working late and would have been in a position to notice the suspicious activity.

### 6. Identifying the Employee Responsible for the Leak

The access log provides the strongest evidence for identifying the insider.

The suspicious operation was:

```text
EXPORT
```

The exported file was:

```text
ROUTE_IT_PL_Q1_2026.pdf
```

The employee responsible was:

```text
BR-0291
```

Additional evidence supports this identification. On the previous day, `BR-0291` had a failed authentication attempt while trying to access the same route document. There was also an external access request associated with a personal email address.

### 7. Identifying the Leaker's Real Name

The final step is to identify the real person behind employee ID `BR-0291`.

Investigate `comms_export.txt`.

A communication references an external Gmail address:

```text
kraliknovak09@gmail.com
```

The address was used in an attempt to access files in the route planning shared folder.

1. Investigate the email address using an OSINT service such as [**Epieos**](https://epieos.com/).

2. The lookup reveals an associated Google profile.

3. The associated Google Maps activity provides additional confirmation of the identity.
