# Ninja Skills

## [Ninja Skills on TryHackMe](https://tryhackme.com/room/ninjaskills)

## Overview

* **Difficulty:** Easy
* **Category:** Linux / File Enumeration
* **Skills Practiced:** Linux file enumeration, file ownership analysis, hashing, content searching, permission analysis, using the `find` command

## Tools & Techniques

* `find` – for locating files based on different criteria
* `grep` – for searching file contents
* `sha1sum` – for calculating SHA1 hashes
* `wc` – for counting lines
* `ls` – for checking file ownership and permissions

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Start the **Ninja Skills** machine on TryHackMe.
* Connect to the machine using the provided AttackBox or your preferred SSH client.

### 2. Find Files Owned by a Specific Group

The first task is to locate all files owned by the group `best-group`.

Run:

```bash
find / -type f -group best-group
```

**Result:**

The command returns the file that belongs to the `best-group` group.

### 3. Locate the Challenge Files

Several tasks involve the same set of files.

Locate them using:

```bash
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) 2>/dev/null
```

The returned files will be used throughout the remaining tasks.

### 4. Find the File Containing an IP Address

Use `grep` together with `find` to search each file for an IPv4 address.

```bash
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec grep -H -E -o "([0-9]{1,3}[\.]){3}[0-9]{1,3}" {} \; 2>/dev/null
```

**Result:**

The output displays the filename together with the IP address it contains.

### 5. Find the File Matching the Given SHA1 Hash

Calculate the SHA1 hash for every challenge file.

```bash
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec sha1sum {} \; 2>/dev/null
```

Compare the generated hashes with the required hash:

```
9d54da7584015647ba052173b84d45e8007eba94
```

**Result:**

The matching filename is the answer.

### 6. Find the File Containing 230 Lines

Count the number of lines in each challenge file.

```bash
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec wc -l {} \; 2>/dev/null
```

**Findings:**

Most files contain **209** lines.

The remaining file contains **230** lines and is the correct answer.

### 7. Find the File Owned by User ID 502

Display the numeric owner and group IDs for each file.

```bash
find / -type f \( -name 8V2L -o -name bny0 -o -name c4ZX -o -name D8B3 -o -name FHl1 -o -name oiMO -o -name PFbD -o -name rmfX -o -name SRSq -o -name uqyw -o -name v2Vb -o -name X1Uy \) -exec ls -ln {} \; 2>/dev/null
```

Review the output and identify the file whose owner has the UID:

```
502
```

### 8. Find the Executable File

The previous command also displays file permissions.

Review the permission bits and identify the file that is executable by everyone.

**Result:**

The file with the execute (`x`) permission set for **owner**, **group**, and **others** is the correct answer.
