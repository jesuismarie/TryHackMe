# Web Application Basics

## [Web Application Basics on TryHackMe](https://tryhackme.com/room/webapplicationbasics)

## Overview

* **Difficulty:** Easy
* **Category:** Web Fundamentals / HTTP & Web Applications
* **Skills Practiced:** Understanding HTTP, URL structure, request/response anatomy, headers, security headers, and practical API calls

## Tools & Techniques

* No external tools needed—this room is theory-focused but includes a practical REST API endpoint for hands-on HTTP practice.
* Familiarity with basic HTTP concepts, a web browser (e.g., Chrome, Firefox), and optional REST clients (like Postman or `curl`) is helpful.

## Walkthrough (Step-by-Step)

### 1. Introduction

* Kick off the room and acknowledge readiness to learn about web applications. No answer needed.

### 2. Web Application Overview

* A **web application** consists of a **front-end** (HTML, CSS, JS – what users interact with) and a **back-end** (servers, databases – where logic and storage live).
* Supporting infrastructure like **web servers** and **WAFs** are essential.

* Key Points:
	* **Web server**: delivers content
	* **Web browser**: consumes and displays content
	* **Web application firewall (WAF)**: filters malicious traffic

### 3. Uniform Resource Locator (URL)

* The URL tells the browser where to go and how to connect. It includes: protocol, domain, path, and sometimes query parameters.
* Attackers can abuse URLs via techniques like typosquatting or malicious query strings.

* Key Points:

	* **HTTPS**: Adds encryption and security
	* **Typosquatting**: Using fake domains that look similar
	* **Query string**: Key-value data appended to URL (e.g., `?id=1`)

### 4. HTTP Messages

* Communication between browser and server happens through **HTTP messages**. Requests are sent by the client, and responses are returned by the server.
* Understand the requests and responses structure: start line, headers, empty line, body.

* Key Points:

	* HTTP = client sends **request**, server sends **response**
	* An **empty line** separates headers from the body

### 5. HTTP Request: Request Line and Methods

* An HTTP request starts with a **request line**. The request line includes methods (GET, POST, DELETE, etc.), path (/page), protocol version (HTTP/1.1).
* Different methods have different purposes (e.g., GET = retrieve data, POST = send data).

* Key Points:

	* **HTTP/1.1**: Widely used protocol version
	* **OPTIONS**: Lists allowed HTTP methods
	* **URL path**: Identifies the specific resource

### 6. HTTP Request: Headers and Body

* Headers add extra metadata like Host, User-Agent, Cookies, etc. The request body often carries data (e.g., form submissions).

* Key Points:

	* **Host** header: Specifies target domain
	* Default form submission type: `application/x-www-form-urlencoded`
	* Metadata is stored in **request headers**

### 7. HTTP Response: Status Line and Status Codes

* When the server replies, it includes a status line: protocol version, status code (numeric), reason phrase (text).
* Codes indicate result: 2xx = success, 3xx = redirection, 4xx = client error, 5xx = server error.

* Key Points:

	* Status line = HTTP version + code + reason phrase
	* **5xx** = Server-side errors
	* **404** = Resource not found

### 8. HTTP Response: Headers and Body

* Responses also carry headers (like cookies, server info) and the actual content (HTML, JSON, images).
* The response can leak sensitive info through headers. Security settings like HttpOnly and Secure flags help protect cookies.

* Key Points:

	* **Server** header: Reveals server software/version (can be risky)
	* `Secure` flag: Cookie sent only via HTTPS
	* `HttpOnly` flag: Prevents JavaScript from reading cookies

### 9. Security Headers

* Security headers strengthen defenses by controlling how browsers handle content.
* CSP (Content-Security-Policy): Controls what scripts can run.
* HSTS: Forces HTTPS.
* nosniff: Stops MIME-type guessing.

* Key Points:

	* `script-src`: Defines allowed script sources (CSP)
	* `includeSubDomains`: HSTS option that forces HTTPS on all subdomains
	* `nosniff`: Stops MIME-type sniffing (prevents drive-by attacks)

### 10. Practical Task: Making HTTP Requests

* Complete three hands-on tasks via the built-in API:

	* **GET** `/api/users` → Flag: `THM{YOU_HAVE_JUST_FOUND_THE_USER_LIST}`
	* **POST** to `/api/user/2`, updating Bob’s country from UK to US → Flag: `THM{YOU_HAVE_MODIFIED_THE_USER_DATA}`
	* **DELETE** `/api/user/1` → Flag: `THM{YOU_HAVE_JUST_DELETED_A_USER}`
