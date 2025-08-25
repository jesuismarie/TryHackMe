# DNS in Detail

## [DNS in Detail on TryHackMe](https://tryhackme.com/room/dnsindetail)

## Overview

* **Difficulty:** Easy
* **Category:** Web Fundamentals / DNS
* **Skills Practiced:** Understanding DNS (Domain Name System) hierarchy, record types, the DNS lookup process, and performing DNS queries

## Tools & Techniques

* Browser + interactive TryHackMe interface for hands-on DNS exploration
* Basic DNS query tools like `nslookup` (simulated in the room)
* Knowledge of DNS architecture, record formats, and caching mechanisms

## Walkthrough (Step-by-Step)

### 1. What Is DNS?

DNS (Domain Name System) translates human-readable domain names into IP addresses—much like using contact names instead of phone numbers—to help us access websites without memorizing numeric addresses.

* Key Points:

	* **DNS = Domain Name System**

### 2. Domain Hierarchy

Domain names are structured from right (TLD) to left (subdomains), with strict rules on length and allowed characters.

* Key Points:

	* Maximum length of a **subdomain**: **63** characters
	* Character not allowed in a subdomain: **_** (underscore)
	* Maximum length of a **full domain name**: **253** characters
	* `.co.uk` is a **ccTLD** (country-code TLD)

### 3. Record Types

Different DNS records serve different purposes: linking domains to IPs, directing emails, aliases, and storing miscellaneous text data.

* Key Points:

	* Record for directing email traffic: **MX**
	* Record for IPv6 addresses: **AAAA**

### 4. Making A Request

When a domain is requested, the system checks multiple levels—from local cache to recursive DNS server, root, TLD, and authoritative servers—until the IP is found and returned. Responses are cached using TTL.

* Key Points:

	* Field that dictates cache duration: **TTL**
	* DNS server typically provided by your ISP: **Recursive** server
	* Server that holds the authoritative records for a domain: **Authoritative** server

### 5. Practical

This section lets you perform simulated DNS queries (like CNAME, TXT, MX, A records) via an interactive interface—mimicking commands such as `nslookup`.

* Key Points:

	* **CNAME** of `shop.website.thm`: **shops.myshopify.com**
	* **TXT** record of `website.thm`: **THM{7012BBA60997F35A9516C2E16D2944FF}**
	* **Priority value** for the **MX** record: **30**
	* **A record** (IP) of `www.website.thm`: **10.10.10.10**
