# Bulletproof Penguin

## [Bulletproof Penguin on TryHackMe](https://tryhackme.com/room/bppenguin)

## Overview

* **Difficulty:** Easy
* **Category:** Linux Hardening / Security Misconfiguration / System Administration
* **Skills Practiced:** Linux service configuration, SSH security, network service hardening, password management, sudo permissions review, insecure protocol mitigation

## Tools & Techniques

* `ssh` – for connecting to the TryHackMe machine
* `vim` – for editing Linux configuration files
* `systemctl` – for restarting and applying service changes
* Redis configuration – disabling unauthenticated access
* SNMP configuration – changing default community strings
* Nginx configuration – correcting service privileges
* inetd configuration – disabling insecure protocols
* SSH hardening – removing weak cryptographic algorithms
* vsftpd configuration – disabling anonymous FTP access
* Linux user management – changing weak passwords and removing unused accounts
* `visudo` – reviewing and modifying sudo permissions
* MySQL and Redis configuration – restricting exposed database services

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Start the **Bulletproof Penguin** machine on TryHackMe.
* Connect to the target machine using SSH.

```bash
ssh thm@<machine-ip>
```

Enter the provided password.

### 2. Redis Server No Password

Redis is running without authentication enabled.

1. Open the Redis configuration file:

```bash
sudo vim /etc/redis/redis.conf
```

2. Search for:

```
requirepass
```

In `vim`, enable password authentication:

```
ESC
/requirepass
```

* Press `i` to enter insert mode.
* Remove the `#` comment character.
* Replace the default password with a strong password.

3. Save and exit:

```
ESC
:wq
```

4. Restart Redis:

```bash
sudo systemctl restart redis-server
```

5. Retrieve the flag:

```bash
get-flags
```

### 3. Report Default Community Names of the SNMP Agent

SNMP is using the default community string.

1. Open the SNMP configuration:

```bash
sudo vim /etc/snmp/snmpd.conf
```

2. Find `rocommunity public default -V systemonly`.

3. Replace `public` with `notpublic`:

4. Save and restart SNMP:

```bash
sudo systemctl restart snmpd
```

5. Retrieve the flag:

```bash
get-flags
```

### 4. Nginx Running as Root

Nginx is configured to run with root privileges.

1. Open the Nginx configuration:

```bash
sudo vim /etc/nginx/nginx.conf
```

2. Find `user root;` and replace it with `user www-data;`.

3. Save the file and restart Nginx:

```bash
sudo systemctl restart nginx
```

4. Retrieve the flag:

```bash
get-flags
```

### 5. Cleartext Protocols

Telnet and TFTP are insecure protocols because they transmit data in plaintext.

1. Open the inetd configuration:

```bash
sudo vim /etc/inetd.conf
```

2. Find the entries for:

```
telnet
tftp
```

3. Disable both services by adding `#` at the beginning of each line.

Example:

```
#telnet ...
#tftp ...
```

4. Save and restart inetd:

```bash
sudo systemctl restart openbsd-inetd
```

5. Retrieve the flag:

```bash
get-flags
```

### 6. Weak SSH Crypto

The SSH server allows weak cryptographic algorithms.

1. Open the SSH configuration:

```bash
sudo vim /etc/ssh/sshd_config
```

2. Remove weak algorithms.

* From `KexAlgorithms`, remove:

	```
	diffie-hellman-group1-sha1
	```

* From `Ciphers`, remove:

	```
	3des-cbc
	aes128-cbc
	aes256-cbc
	```

* From `MACs`, remove:

	```
	hmac-md5-96
	```

3. Save and restart SSH:

```bash
sudo systemctl restart sshd
```

4. Retrieve the flag:

```bash
get-flags
```

### 7. Anonymous FTP Login Reporting

Anonymous FTP access is enabled.

1. Open the FTP configuration:

```bash
sudo vim /etc/vsftpd.conf
```

2. Find `anonymous_enable=YES` and change it to `anonymous_enable=NO`.

3. Save and restart FTP:

```bash
sudo systemctl restart vsftpd
```

4. Retrieve the flag:

```bash
get-flags
```

### 8. Weak Passwords

Some users have weak passwords.

1. Change passwords for `mary` and `munra`.

* Update Mary's password:

	```bash
	sudo passwd mary
	```

* Update Munra's password:

	```bash
	sudo passwd munra
	```

2. Remove unused accounts:

* Delete `joseph`:

	```bash
	sudo userdel joseph
	```

* Delete `test1`:

	```bash
	sudo userdel test1
	```

3. Retrieve the flag:

```bash
get-flags
```

### 9. Review Sudo Permissions

1. Review current sudo permissions:

```bash
sudo visudo
```

2. Find and remove:

```
munra ALL=(ALL:ALL) ALL
```

3. Add restricted sudo permission for Mary:

```
mary ALL=(ALL) NOPASSWD: /usr/bin/ss
```

4. Save and exit.

5. Retrieve the flag:

```bash
get-flags
```

### 10. Exposed Database Ports

Database services are listening on external interfaces.

#### MySQL

1. Open MySQL configuration:

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

2. Find `bind-address` and change value to `127.0.0.1`.

3. Restart MySQL:

```bash
sudo systemctl restart mysql
```

#### Redis

1. Open Redis configuration:

```bash
sudo vim /etc/redis/redis.conf
```

2. Find `bind-address` and change value to `127.0.0.1`.

3. Restart Redis:

```bash
sudo systemctl restart redis-server
```

4. Retrieve the final flag:

```bash
get-flags
```
