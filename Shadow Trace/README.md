# ShadowTrace

## [ShadowTrace on TryHackMe](https://tryhackme.com/room/shadowtrace)

## Overview

* **Difficulty:** Easy
* **Category:** Malware Analysis / Digital Forensics / Threat Detection
* **Skills Practiced:** Static malware analysis, PE file analysis, string extraction, Base64 decoding, process investigation, malicious URL identification

## Tools & Techniques

* **PeStudio** – for static analysis of Windows executables
* `strings` – for extracting readable strings from binaries
* `findstr` – for searching extracted strings
* **CyberChef** – for decoding encoded strings and extracting indicators
* **Dashboard** – for investigating suspicious process alerts
* **PowerShell process analysis** – for identifying malicious commands and URLs
* **Chrome process analysis** – for decoding encoded data and identifying URLs

## Walkthrough (Step-by-Step)

### 1. Static Analysis of `windows-update.exe`

1. Navigate to **DFIR Tools** and open **PeStudio**.

2. Open the following file:

```text
windows-update.exe
```

3. Review the file information.

**Findings:**

PeStudio provides information including:

* File type
* SHA256 hash
* Indicators associated with the executable

4. In the **Indicators** section, look for a URL associated with the executable.

5. To identify the domain, use `strings` together with `findstr`:

```cmd
strings windows-update.exe | findstr <domain>
```

The command reveals the domain contained in the executable.

### 2. Decode the Embedded String

The executable also contains an encoded string.

1. Copy the encoded value from the PeStudio analysis.

2. Open [**CyberChef**](https://gchq.github.io/CyberChef/)

3. Decode the string to obtain the flag.

### 3. Identify the Socket Library

1. Review the **Libraries** section in PeStudio.

2. Look for libraries related to network socket functionality.

**Finding:**

A library associated with socket/network communication is present in the executable. This indicates that the malware may be capable of communicating over the network.

### 4. Investigating the PowerShell Alert

1. Open the **Dashboard**.

2. Locate the alert associated with:

```text
powershell.exe
```

3. Inspect the PowerShell command or associated alert data.

The alert contains an encoded string.

4. Decode the string using CyberChef to reveal a malicious URL.

### 5. Investigating the Chrome Process

1. Inspect the alert associated with:

```text
chrome.exe
```

The alert contains an encoded array.

2. Copy the encoded array.

3. Decode it using CyberChef.

The decoded array contains a URL associated with the malicious activity.

### 6. Identifying the Dropped File

Review the `chrome.exe` alert information.

The alert contains the path of the file that was saved to the system.
