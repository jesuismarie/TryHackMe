# Water Bottle

## [Water Bottle on TryHackMe](https://tryhackme.com/room/waterbottle)

## Overview

* **Difficulty:** Easy
* **Category:** OSINT / Geolocation / Historical Investigation
* **Skills Practiced:** Google Maps investigation, historical Street View analysis, business identification, phone number enumeration, location-based OSINT

## Tools & Techniques

* **Google Maps** – for locating water refilling stations around Boni Avenue
* **Google Earth** – for reviewing historical imagery
* **Google Search** – for searching business names and contact information

## Walkthrough (Step-by-Step)

### 1. Understanding the Challenge

The objective is to identify a water refilling station that existed until **2014**.

The available clues are:

* The location is near **Boni Avenue**.
* The original water station no longer exists.
* A new water refilling establishment is currently located at the same place.
* The original station's contact number starts with:

```text
63922
```

* The complete contact number contains **12 digits**.

The flag format is:

```text
THM{waterstationname_contactnumber}
```

### 2. Identifying the Location

Start by searching for water refilling stations around **Boni Avenue**.

Open Google Maps and search for:

```text
water refilling station Boni Avenue
```

The relevant area is **Mandaluyong, Metro Manila, Philippines**.

Instead of only checking currently listed businesses, compare the locations with older imagery because the required station existed until 2014.

### 3. Checking Historical Street View

1. Open Google Maps and locate water refilling stations around Boni Avenue.

2. Select a possible location and open **Street View**.

3. Use the historical imagery option:

```text
See more dates
```

4. Change the imagery date to around **2014**.

5. Compare the business visible in the historical image with the current business at the same location.

One of the locations currently contains:

```text
Alkafresco Water Refilling Station
```

However, historical imagery shows that a different water station occupied the location in 2014.

### 4. Identifying the Original Water Station

The historical business name is:

```text
Aquabest
```

The name can then be searched together with the known phone number prefix:

```text
Aquabest 63922
```

Search results reveal the original station's contact number.

**Finding:**

```text
Aquabest
639228721288
```

The phone number matches the clue because it:

* Contains 12 digits.
* Starts with `63922`.
