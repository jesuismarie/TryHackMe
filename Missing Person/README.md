# Missing Person

## [Missing Person on TryHackMe](https://tryhackme.com/room/missingperson)

## Overview

* **Difficulty:** Easy
* **Category:** OSINT / Investigation
* **Skills Practiced:** Image analysis, reverse image search, metadata extraction, geolocation, social media investigation, event research, business information gathering

## Tools & Techniques

* `exiftool` – for extracting metadata from images
* **Google Lens** – for identifying locations and restaurants
* **Google Search** – for researching the MotoGP event and after-party
* **Google Maps** – for verifying the bar's location

## Walkthrough (Step-by-Step)

### 1. Download Files

1. Download the ZIP file provided by the room.

2. Extract the archive.

The archive contains two images:

```text
MotoGP.jpg
food.jpg
```

The goal is to use the information contained in these images and the provided message to reconstruct the missing person's movements.

### 2. Identifying the MotoGP Circuit

The first image shows a MotoGP race.

1. Perform a reverse image search using Google Image Search.

2. The image leads to the MotoGP event held at the **Pertamina Mandalika International Street Circuit** in Indonesia.

**Finding:**

```text
Pertamina Mandalika International Street Circuit
```

### 3. Identifying the Event Date

Now that the circuit has been identified, search for the 2025 MotoGP event held at the circuit.

The event took place from:

```text
03-05/10/2025
```

The image metadata can also be inspected with `exiftool`:

```bash
exiftool MotoGP.jpg
```

The metadata indicates that the image was taken on **5 October 2025**, which corresponds to the final day of the event.

### 4. Identifying the Mexican Restaurant

The second image shows the food that the missing person ate.

1. Perform a reverse image search on `food.jpg`.

2. The image can be identified as being taken at **Cantina Mexicana** in Kuta Lombok, Indonesia.

**Finding:**

```text
Cantina Mexicana
```

### 5. Finding the Time the Photo Was Taken

The exact time can be obtained from the image metadata.

Run:

```bash
exiftool food.jpg
```

Look for the original date and time information.

**Finding:**

```text
19:55:30
```

The restaurant photo was taken at **19:55:30**.

### 6. Finding the After-Party Bar

The missing person sent the following message:

```text
Went to this cool MotoGP after party, and became friends with one of the local DJs who played that night. We're going to visit a cave tomorrow.
```

The message specifically says that the after-party was at a **bar**.

1. Search for MotoGP after-parties in Indonesia around the event date.

2. After investigating the available events, identify **Surfer's Bar** in Kuta Lombok.

3. Use Google Maps to verify the location and obtain the full address.

**Finding:**

```text
Surfer's Bar
Jl. Raya Kuta, Kuta, Kec. Pujut, Kabupaten Lombok Tengah, Nusa Tenggara Barat
```

The distinction between a bar and a beach club is important when searching for the correct event.

### 7. Identifying the DJ

Investigate the social media posts and event information associated with the after-party.

The DJ who played at the event can be identified from the event information.

**Finding:**

```text
Bong Leleh
```

The DJ's stage name is **Bong Leleh**.

### 8. Finding the Cave

The message states that the missing person planned to visit a cave with the DJ the following day.

Use the DJ's name and the location of Surfer's Bar to search for nearby caves and tourist activities.

The investigation leads to:

```text
Gua Sumur
```

**Finding:**

```text
Gua Sumur
```

Bong Leleh is associated with guiding tourists to this cave.

### 9. Finding the DJ's Old Business Phone Number

The final task is to find the phone number associated with the DJ's old business.

1. Search for **Bong Leleh** and investigate his online presence.

2. Look through social media and business information connected to the cave tours.

3. The phone number associated with the old business is:

```text
85333137345
```

The room requires the full number **without the country code**, so the answer is:

```text
085333137345
```

This number is associated with Bong Leleh's tourism business.
