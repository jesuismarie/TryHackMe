# Grep

## [Grep on TryHackMe](https://tryhackme.com/room/greprtp)

## Overview

* **Difficulty:** Medium
* **Category:** Web Exploitation / Privilege Escalation / CTF
* **Skills Practiced:** Port enumeration, virtual host discovery, API key discovery, Burp Suite interception, directory enumeration, file upload bypass, reverse shell, database backup analysis, subdomain enumeration

## Tools & Techniques

* `nmap` – for port and service enumeration
* `/etc/hosts` – for configuring local hostname resolution
* **Burp Suite** – for intercepting and modifying HTTP requests
* `gobuster` – for directory enumeration
* `hexedit` – for modifying file magic bytes
* `nc` – for receiving the reverse shell
* `find` – for locating database backup files

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Start the **Grep** machine on TryHackMe.
* Obtain the target IP address.

### 2. Initial Enumeration

Run a full TCP port scan with service detection:

```bash
nmap -p- -sV <target-ip>
```

**Findings:**

```text
22/tcp     open  ssh      OpenSSH 8.2p1 Ubuntu
80/tcp     open  http     Apache httpd 2.4.41
443/tcp    open  ssl/http Apache httpd 2.4.41
51337/tcp  open  http     Apache httpd 2.4.41
```

The HTTPS service is particularly interesting because its certificate contains a hostname.

### 3. Discovering the Virtual Host

Visit:

```text
https://<target-ip>
```

Inspect the TLS certificate.

**Finding:**

The certificate's Common Name (CN) is:

```text
grep.thm
```

Add the hostname to `/etc/hosts`:

```bash
sudo vim /etc/hosts
```

Add:

```text
<target-ip> grep.thm
```

Save the file and visit:

```text
https://grep.thm
```

### 4. Registering an Account

Open **Burp Suite** and configure the browser to use Burp as the proxy.

1. Navigate to the registration page.
2. Intercept the registration request.
3. Inspect the request and application behavior.

The website references a project called:

```text
SearchMe
```

Search for the source code online:

```text
"SearchMe" php site:github.com
```

A GitHub repository containing the application's source code can be found.

### 5. Finding the API Key

Inspect the GitHub repository and its commit history.

The required API key can be found in one of the commits.

Copy the API key and add it to the intercepted registration request in Burp Suite.

Forward the modified request.

**Result:**

```text
Registration Successfully
```

The account has been successfully registered.

### 6. Login and Initial Flag

Log in to the newly created account.

After successful authentication, the website provides the first flag.

### 7. Directory Enumeration

Enumerate the web application directory:

```bash
gobuster dir -u https://grep.thm/public/html/ \
-w /usr/share/wordlists/dirb/common.txt \
-k -x php
```

**Discovered:**

```text
admin.php
dashboard.php
index.php
login.php
logout.php
register.php
upload.php
```

Some pages return `403 Forbidden`, while the main application pages are accessible.

### 8. Exploiting the File Upload

Navigate to:

```text
https://grep.thm/public/html/upload.php
```

Attempt to upload a PHP reverse shell.

The application rejects the file because it only allows:

```text
jpg
jpeg
png
bmp
```

Inspect the source code found earlier.

The application checks both the extension and the file's magic bytes:

```php
$allowedExtensions = ['jpg', 'jpeg', 'png', 'bmp'];

$validMagicBytes = [
    'jpg' => 'ffd8ffe0',
    'png' => '89504e47',
    'bmp' => '424d'
];
```

Therefore, simply changing the file extension is not enough.

### 9. Bypassing the Magic Byte Check

Modify the beginning of the reverse-shell file so that it contains valid image magic bytes.

Use `hexedit` to modify the file header.

After modifying the bytes, upload the file again.

**Result:**

The file is successfully uploaded.

### 10. Getting a Reverse Shell

Start a Netcat listener on the attacker machine:

```bash
nc -lvnp <port>
```

Open the uploaded file through:

```text
https://grep.thm/public/html/uploads/<uploaded-file>
```

The uploaded file executes and connects back to the Netcat listener.

**Result:**

A reverse shell is obtained on the target machine.

### 11. Finding the Database Backup

Search the filesystem for SQL files:

```bash
find / -name "*.sql" 2>/dev/null
```

**Finding:**

```text
/var/www/backup/users.sql
```

Read the database backup:

```bash
cat /var/www/backup/users.sql
```

The file contains information about the application's users, including the administrator's email address.

### 12. Discovering the LeakChecker Application

Inspect the `/var/www` directory:

```bash
ls -la /var/www
```

Several web applications/directories are present, including:

```text
leakchecker
```

The target is also running an HTTP service on port `51337`.

Add the `leakchecker` hostname to `/etc/hosts`:

```bash
sudo vim /etc/hosts
```

Add:

```text
<target-ip> leakchecker.grep.thm
```

Save the file.

### 13. Finding the Administrator Password

Access the LeakChecker application:

```text
https://leakchecker.grep.thm:51337
```

The application exposes information that can be used to obtain the administrator's password.

**Result:**

The administrator's password is revealed through the **LeakChecker** application.
