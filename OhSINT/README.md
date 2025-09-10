# OhSINT

## [OhSINT on TryHackMe](https://tryhackme.com/room/ohsint)

## Overview

* **Difficulty:** Easy / Beginner
* **Category:** OSINT (Open-Source Intelligence)
* **Skills Practiced:**

	* Extracting metadata from image files
	* Using online search, social media and blogging platforms for reconnaissance
	* Leveraging tools like Wigle.net for Wi-Fi mapping
	* Inspecting web page source code to reveal hidden information

## Tools & Techniques

* **exiftool** (or web-based metadata extractors) – to extract metadata from images, including GPS coordinates and copyright info
* **Search Engines** – using the extracted author/username to find associated social media profiles, GitHub, and blogs
* **Wigle.net** – to lookup Wi-Fi BSSID and derive SSID and location information
* **Browser “View Source” / Developer Tools** – to uncover hidden text (e.g., password hidden in white font)

## Walkthrough (Step-by-Step)

### 1. Inspect Image Metadata

* Download the task files and extract `WindowsXP*.jpg`.
* Run:

	```bash
	exiftool WindowsXP_*.jpg
	```

	**Findings:**

	* GPS Coordinates: `54°17'41.27" N, 2°15'1.33" W`
	* Copyright: `OWoodflint`

### 2. Identify User's Avatar

* Search "OWoodflint" via Google.
* Visit found Twitter/X profile.

	**Findings:**

	* Avatar is a **cat**.

### 3. Find User's Location via BSSID

* On the Twitter/X profile, locate BSSID:

	```
	BSSID: B4:5D:50:AA:86:41
	```
* Go to Wigle.net, input the BSSID and view location on map.

	**Findings:**

	* **City:** London
	* **SSID of WAP:** `UnileverWiFi`

### 4. Find Personal Email and Source

* Visit GitHub page linked to “OWoodflint”.

	**Findings:**

	* Email: `OWoodflint@gmail.com`
	* Found on GitHub.

### 5. Discover Holiday Destination

* Visit their WordPress blog.

	**Findings:**

	* Mentioned holiday location: **New York**.

### 6. Uncover Hidden Password

* Open WordPress blog source code or view page source.

	**Findings:**

	* Password: `pennYDr0pper.!` hidden in white text via `<p style="color:#ffffff;">`.
