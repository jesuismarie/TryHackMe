# Digital Footprint

## [Digital Footprint on TryHackMe](https://tryhackme.com/room/osintchallengeiv)

## Overview

* **Difficulty:** Easy
* **Category:** OSINT / Geolocation / Digital Forensics
* **Skills Practiced:** Image metadata analysis, GPS geolocation, web archive investigation, reverse image search, Google Earth research, social media investigation

## Tools & Techniques

* `exiftool` – for extracting metadata and GPS coordinates from images
* Google Maps – for identifying locations from GPS coordinates
* Internet Archive – for investigating historical versions of websites
* Google Lens – for reverse image searching
* Google Earth – for identifying buildings and locations
* Social media search – for identifying accounts from extracted usernames

## Walkthrough (Step-by-Step)

### 1. Finding the Location from Image Metadata

1. Download the file provided by the room.

2. Use `exiftool` to inspect the image metadata:

```bash
exiftool path/to/image
```

3. Look for the GPS coordinates in the output.

4. Open [**Google Maps**](https://www.google.com/maps)

5. Enter the GPS coordinates into Google Maps.

6. Change the map orientation to the **South** and inspect the surrounding area.

**Result:**

The location reveals the city required for the task.

### 2. Investigating the Historical Website

The second task requires finding information about an old version of a website.

1. Open the [**Internet Archive**](https://archive.org/)

2. Search for:

```text
warc-acme.com/jef/
```

No useful result is found using the complete path.

3. Remove `/jef` from the search query and search for:

```text
warc-acme.com
```

4. Review the archived website captures.

**Finding:**

The Internet Archive reveals the first day on which the website was published.

**Result:**

The publication date is the answer for this task.

### 3. Identifying the Building

1. Download the image provided for the third task.

2. Use **Google Lens** to perform a reverse image search.

3. Use the information from the image search to identify the general location.

4. Open [**Google Earth**](https://earth.google.com/) and navigate to the identified area.

5. Look for the building visible on the **right side** of the image.

6. Identify the building and check its name.

**Result:**

The name of the building provides the answer for the task.

### 4. Identifying the Social Media Account

1. Download the file provided for the fourth task.

2. Inspect its EXIF metadata:

```bash
exiftool path/to/image
```

3. Look for a username in the metadata.

**Finding:**

```text
markwilliams7243
```

4. Search for the username across social media platforms.

5. The search reveals a **YouTube** account associated with the username.

6. Investigate the YouTube account and inspect its content.

**Result:**

The required flag can be found on the discovered YouTube account.
