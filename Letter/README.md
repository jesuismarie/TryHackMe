# Letter

## [Letter on TryHackMe](https://tryhackme.com/room/letter)

## Overview

* **Difficulty:** Easy
* **Category:** OSINT / Historical Investigation / Steganography
* **Skills Practiced:** Image analysis, postal barcode decoding, historical research, newspaper analysis, date identification, OSINT investigation

## Tools & Techniques

* **French Postal Barcode Decoder** – for decoding the barcode found on the envelope
* **La Poste** – for verifying the postal code
* **Historical newspaper research** – for identifying the approximate date
* **Search engines** – for investigating historical events and identifying the intended recipient

## Walkthrough (Step-by-Step)

### 1. Download Files

* Download the file provided by the **Letter** room.
* Open the image and inspect the envelope.

### 2. Decoding the Postal Barcode

The envelope contains a barcode that can be used to identify the postal code.

1. Open the [**French Postal Barcode Decoder**](https://www.dcode.fr/french-postal-barcode).

2. Decode the barcode found on the envelope.

**Finding:**

The decoded barcode provides the postal code.

3. Verify the postal code using the [French postal service](https://www.laposte.fr/outils/resultat-test-code-postal) website:

### 3. Identifying the Date

The newspaper visible in the image provides additional information that can be used to determine the approximate date of the letter.

1. Examine the newspaper and identify the information printed on it.

2. The weekday starts with **D**, which narrows the possibilities to **Dimanche (Sunday)**.

3. Use the newspaper's name and other visible information to search for historical editions and determine the approximate period.

**Finding:**

The newspaper corresponds to:

```text
1925-05-24
```

### 4. Identifying the Intended Person

Search for the historical event associated with the newspaper date.

```text
la catastrophe du 23 mai 1925
```

**Finding:**

The search leads to an article about the [**23 May 1925 catastrophe in Penmarc'h**](https://kbcpenmarch.franceserv.com/la-catastrophe-du-23-mai-1925-selon-les-annales-du-sauvetage.html).

The article describes the maritime disaster and provides information about the people involved.

Review the names mentioned in the article to identify the person the letter was intended for.

**Result:**

The intended person's name can be found in the historical records associated with the catastrophe.
