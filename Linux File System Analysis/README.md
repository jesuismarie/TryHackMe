# Linux Filesystem Analysis

## [Linux Filesystem Analysis on TryHackMe](https://tryhackme.com/room/linuxfilesystemanalysis)

## Overview

* **Difficulty:** Easy → Medium
* **Category:** Forensics / Linux filesystem analysis
* **Skills Practiced:** Forensic file/timestamp analysis, permissions & ownership inspection, binary integrity checks, basic rootkit detection

## Tools & Techniques

* `ssh` — connect to the machine (or use the AttackBox)
* `find`, `stat`, `ls`, `cat` — filesystem inspection
* `exiftool` — file type/metadata inspection
* `debsums`, `md5sum` — package / binary integrity checks
* `chkrootkit`, `rkhunter` — basic rootkit detection
* `getent`, `cut`, `grep` — users/groups enumeration
* `sudo` — run restricted commands when appropriate

## Walkthrough (Step-by-Step)

> Deploy the machine and connect with SSH (or open the machine in the AttackBox).
> Credentials supplied by the room:
> **Username:** `investigator`
> **Password:** `TryHackMe123!`

### 1. Investigation Setup

**Steps / Commands:**

```bash
export PATH=/mnt/usb/bin:/mnt/usb/sbin
export LD_LIBRARY_PATH=/mnt/usb/lib:/mnt/usb/lib64
check-env
```

**Output**

```
THM{5514ec4f1ce82f63867806d3cd95dbd8}
```

### 2. Files, Permissions, and Timestamps

1. Find recently changed files owned by `bob` (recent 1 minute):

	```bash
	find / -user bob -type f -cmin -1 2>/dev/null
	```

	**Output**

	```
	THM{0b1313afd2136ca0faafb2daa2b430f3}
	```

2. Inspect suspicious ELF in web assets:

	```bash
	exiftool /var/www/html/assets/reverse.elf
	```

	**Findings:**

	```
	File Type: application/octet-stream
	```

3. File timestamp inspection (hosts file):

	```bash
	stat /etc/hosts
	```

	**Findings (excerpt):**

	```
	Modify: 2020-10-26 21:10:44.000000000 +0000
	```

### 3. Users and Groups

1. Find users with UID 0 (root-equivalent):

	```bash
	cat /etc/passwd | cut -d: -f1,3 | grep ':0$'
	```

	**Findings:**

	```
	root:0
	b4ckd00r3d:0
	```

2. Query group info (example: plugdev gid 46):

	```bash
	getent group 46
	```

	**Findings:**

	```
	plugdev
	```

3. Check sudoers for specific user privileges (jane):

	```bash
	sudo cat /etc/sudoers | grep jane
	```

	**Findings:**

	```
	jane   ALL=(ALL) /usr/bin/pstree
	```

### 4. User Directories and Files

1. Inspect Jane’s bash history (requires sudo):

	```bash
	sudo cat /home/jane/.bash_history
	```

	**Findings (flag):**

	```
	THM{f38279ab9c6af1215815e5f7bbad891b}
	```

2. Check Bob’s home for hidden files:

	```bash
	ls -la /home/bob
	cat /home/bob/.hidden34
	```

	**Findings (flag):**

	```
	THM{6ed90e00e4fb7945bead8cd59e9fcd7f}
	```

3. Check Jane’s authorized_keys timestamp:

	```bash
	sudo stat /home/jane/.ssh/authorized_keys
	```

	**Findings (excerpt):**

	```
	Modify: 2024-02-13 00:34:16.005897449 +0000
	```

### 5. Binaries and Executables

1. Check package file integrity (debsums):

	```bash
	sudo debsums -e -s
	```

	**Findings (excerpt):**

	```
	debsums: changed file /etc/sudoers (from sudo package)
	```

2. Inspect suspicious binary in /var/tmp:

	```bash
	md5sum /var/tmp/bash
	```

	**Findings (example):**

	```
	7063c3930affe123baecd3b340f1ad2c  /var/tmp/bash
	```

### 6. Rootkits

1. chkrootkit scan (search for suspicious scripts):

	```bash
	sudo chkrootkit | grep ".sh"
	```

	**Findings:**

	```
	/var/tmp/findme.sh
	```

2. rkhunter system check (accounts / warnings):

	```bash
	sudo rkhunter -c -sk | grep accounts
	```

	**Findings / Note:**

	```
	Warning
	```
