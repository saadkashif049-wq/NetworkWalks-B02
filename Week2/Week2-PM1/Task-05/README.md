# Task 5 — HTTP Header Analysis with `curl`

![Week](https://img.shields.io/badge/Week-02-blue?style=flat-square )
![Module](https://img.shields.io/badge/Module-PM1%20%7C%20Footprinting-orange?style=flat-square )
![Task](https://img.shields.io/badge/Task-04%20%7C%20curl-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square )
![Purpose](https://img.shields.io/badge/Purpose-Educational%20Only-yellow?style=flat-square )

> **Project:** Week 2 — Project Module 1: Footprinting and Reconnaissance
>
> **Objective:** Read the HTTP response headers of `networkwalks.com` to identify the server response, status code, cookies, redirects, caching information, and publicly exposed technologies.
>
> **Authorization:** Perform reconnaissance only against websites and systems you own or have explicit permission to assess.

---

## Table of Contents

- [What Is `curl`?](#what-is-curl)
- [How It Works](#how-it-works)
- [Command Syntax](#command-syntax)
- [Step-by-Step: Running the Task](#step-by-step-running-the-task)
- [Reading the Output](#reading-the-output)
- [Important HTTP Headers](#important-http-headers )
- [Redirects, Cookies, and Hidden Endpoints](#redirects-cookies-and-hidden-endpoints)
- [🔴 Attacker Perspective](#-attacker-perspective)
- [🔵 Defender Perspective](#-defender-perspective)
- [Conclusion](#conclusion)

---

## What Is `curl`?

`curl` is a command-line tool used to transfer data to and from servers using network protocols such as HTTP and HTTPS.

For this task, the `-I` option is used to request only the HTTP response headers instead of downloading the complete webpage.

The required command is:

```bash
curl -I https://networkwalks.com
```

HTTP headers can reveal useful information about how a web server handles requests and responses, including:

- HTTP status codes.
- Web-server banners.
- Redirect locations.
- Cookies.
- Cache behavior.
- Content types.
- Security headers.
- Proxy and CDN information.
- Publicly visible technology indicators.

> **Important:** Header information is useful for fingerprinting, but a single response does not always reveal the complete web stack.

---

## How It Works

When the command is executed, `curl` establishes an HTTPS connection to the target and sends a request asking for the response headers.

### Request Flow

```text
Kali Linux terminal
        |
        |  curl -I https://networkwalks.com
        v
HTTPS connection to the target
        |
        |  Sends a HEAD request
        v
Web server processes the request
        |
        |  Returns HTTP response headers
        v
curl displays the headers in the terminal
```

The `-I` option requests the headers without retrieving the complete response body. This makes it useful for quickly observing the server's response behavior.

The command may reveal information from the web server, reverse proxy, CDN, caching system, application framework, or security middleware.

---

## Command Syntax

### Basic Syntax

```text
curl [options] URL
```

Required command:

```bash
curl -I https://networkwalks.com
```

### Useful Variations

| Command | Purpose |
|---|---|
| `curl -I https://networkwalks.com` | Request HTTP response headers only. |
| `curl -i https://networkwalks.com` | Display headers together with the response body. |
| `curl -L -I https://networkwalks.com` | Follow redirects and display headers from the redirect chain. |
| `curl -v -I https://networkwalks.com` | Display detailed connection and request information. |
| `curl -sS -I https://networkwalks.com` | Run quietly while still showing errors. |
| `curl -k -I https://networkwalks.com` | Ignore certificate verification errors; use only in an authorized lab. |
| `curl -A "Mozilla/5.0" -I https://networkwalks.com` | Send a custom User-Agent header. |

### Recommended Evidence Command

Display the headers and save them to a file:

```bash
curl -I https://networkwalks.com | tee task-4-curl-headers.txt
```

To follow redirects and save the complete header chain:

```bash
curl -L -I https://networkwalks.com | tee task-4-curl-redirects.txt
```

---

## Step-by-Step: Running the Task

### 1. Open the Terminal

Launch a terminal in Kali Linux.

### 2. Confirm That `curl` Is Available

```bash
which curl
```

### 3. Run the Required Command

```bash
curl -I https://networkwalks.com
```

### 4. Save the Output

```bash
curl -I https://networkwalks.com | tee task-4-curl-headers.txt
```

### 5. Check for Redirects

```bash
curl -L -I https://networkwalks.com
```

The `-L` option follows redirects and displays the final response. This can help identify whether the site redirects between HTTP and HTTPS, between domains, or between different paths.

### 6. Capture Evidence

Take a screenshot showing:

- The Kali Linux terminal.
- The command that was executed.
- The complete HTTP header output.
- The terminal prompt or timestamp, if available.

### 7. Organize the Evidence

```text
Task-4-curl/
├── README.md
├── task-4-curl-headers.txt
├── task-4-curl-redirects.txt
└── screenshots/
    └── curl-networkwalks.png
```

---

## Reading the Output

A typical response may look similar to this:

```text
HTTP/2 200
server: Apache
content-type: text/html; charset=UTF-8
cache-control: max-age=600
set-cookie: example=value; Secure; HttpOnly
x-powered-by: PHP
link: <https://networkwalks.com/wp-json/>; rel="https://api.w.org/"
```

The exact response depends on the current website configuration, web server, proxy, CDN, and application state.

| Header or Output | Meaning |
|---|---|
| `HTTP/2 200` | The request succeeded and the server returned a successful response. |
| `server: Apache` | The response identifies Apache as the web server. |
| `content-type` | Indicates the type and character encoding of the returned content. |
| `cache-control` | Defines how browsers and intermediate systems should cache the response. |
| `set-cookie` | Instructs the client to store a cookie. |
| `location` | Identifies the destination of a redirect. |
| `x-powered-by` | May disclose the application runtime or framework. |
| `link` | May expose related resources, API endpoints, or application metadata. |

---

## Important HTTP Headers

### HTTP Status Code

The first line contains the HTTP version and status code.

| Status Code | General Meaning |
|---|---|
| `200 OK` | The request succeeded. |
| `301 Moved Permanently` | The resource has permanently moved to another location. |
| `302 Found` | The server is temporarily redirecting the request. |
| `403 Forbidden` | The server understood the request but refuses access. |
| `404 Not Found` | The requested resource could not be found. |
| `429 Too Many Requests` | The client has sent too many requests in a given period. |
| `500 Internal Server Error` | The server encountered an unexpected error. |
| `503 Service Unavailable` | The service is temporarily unavailable. |

### `Server`

The `Server` header may reveal the web-server software. For example:

```text
server: Apache
```

Server banners can help defenders identify unnecessary information disclosure and can help authorized testers understand the public technology stack.

### `Location`

The `Location` header identifies where a client should go after a redirect.

Example:

```text
location: https://www.networkwalks.com/
```

Redirects may reveal canonical hostnames, HTTPS enforcement, login paths, or application routing behavior.

### `Set-Cookie`

The `Set-Cookie` header instructs the browser to store a cookie.

Important cookie attributes include:

| Attribute | Purpose |
|---|---|
| `Secure` | Sends the cookie only over HTTPS. |
| `HttpOnly` | Prevents ordinary client-side JavaScript from reading the cookie. |
| `SameSite` | Helps control cross-site cookie behavior. |
| `Domain` | Defines which domain can receive the cookie. |
| `Path` | Defines which URL paths can receive the cookie. |

### `Cache-Control`

The `Cache-Control` header defines caching behavior for browsers, proxies, and other intermediate systems.

Examples include:

```text
cache-control: no-store
cache-control: max-age=600
cache-control: public
```

### `Content-Type`

The `Content-Type` header identifies the type of content returned by the server.

Example:

```text
content-type: text/html; charset=UTF-8
```

### Security Headers

The response may contain security-related headers such as:

- `Strict-Transport-Security`.
- `Content-Security-Policy`.
- `X-Content-Type-Options`.
- `X-Frame-Options`.
- `Referrer-Policy`.
- `Permissions-Policy`.

The presence, absence, and configuration of these headers can help indicate the security maturity of a web application.

---

## Redirects, Cookies, and Hidden Endpoints

### Redirect Analysis

Use the following command to view redirects:

```bash
curl -L -I https://networkwalks.com
```

A redirect chain may show:

- HTTP-to-HTTPS enforcement.
- A redirect from a non-canonical hostname.
- A CDN or proxy endpoint.
- Login or authentication paths.
- Application routing behavior.

### Cookie Analysis

Look for headers such as:

```text
set-cookie: session=value; Secure; HttpOnly; SameSite=Lax
```

Cookies should be reviewed for secure attributes, session identifiers, tracking behavior, and unnecessarily broad domain or path scope.

Do not reuse, modify, or attack session cookies unless this is explicitly authorized in the rules of engagement.

### Publicly Exposed Endpoints

HTTP response headers can expose links to related resources or application APIs. The task material highlights the possibility of a WordPress REST API reference such as:

```text
/wp-json/
```

A visible endpoint is not automatically a vulnerability. It should be documented as an observation and tested only when the assessment scope explicitly permits it.

---

## 🔴 Attacker Perspective

From an attacker’s perspective, HTTP headers provide a quick way to fingerprint the web stack and identify how the application is configured.

During an **authorized security assessment**, the results may be used to:

- Identify the web-server platform.
- Determine the HTTP version in use.
- Observe redirects and canonical hostnames.
- Identify cookies and session-management attributes.
- Identify caching behavior and proxy infrastructure.
- Detect application frameworks or runtime information.
- Locate publicly referenced endpoints such as `/wp-json/`.
- Compare exposed technologies with approved vulnerability information.

Headers can reveal useful information without downloading the complete webpage. However, a header observation is not proof that the detected server or application is vulnerable.

> **Safety boundary:** Do not access hidden endpoints, manipulate cookies, bypass access controls, or exploit detected technologies without explicit authorization.

---

## 🔵 Defender Perspective

Defenders should inspect their own HTTP headers to determine whether the web application discloses unnecessary information or lacks important security controls.

A defensive review should consider:

- Whether the `Server` header exposes unnecessary version information.
- Whether `X-Powered-By` reveals the application runtime.
- Whether cookies use `Secure`, `HttpOnly`, and appropriate `SameSite` attributes.
- Whether HTTPS is enforced correctly.
- Whether redirects lead only to trusted destinations.
- Whether cache settings protect sensitive information.
- Whether security headers are present and correctly configured.
- Whether public API endpoints expose more information than intended.

| Finding | Recommended Defensive Action |
|---|---|
| Server version disclosed | Minimize unnecessary web-server banner information. |
| Runtime disclosed through `X-Powered-By` | Remove or reduce the header where appropriate. |
| Cookie missing `Secure` | Send the cookie only over HTTPS. |
| Cookie missing `HttpOnly` | Protect sensitive session cookies from client-side script access. |
| Weak `SameSite` setting | Review cross-site cookie behavior and application requirements. |
| Missing HSTS | Configure HTTPS enforcement where appropriate. |
| Missing content-security controls | Review and implement suitable security headers. |
| Public endpoint exposed unnecessarily | Restrict, authenticate, or remove the endpoint where appropriate. |

> **Defensive goal:** Return only the information required for normal operation and configure headers, cookies, redirects, and caching securely.

---

## Conclusion

The `curl -I` command provides a fast way to inspect HTTP response headers without downloading the complete webpage. For `networkwalks.com`, the output can reveal the response status, server banner, cookies, redirects, caching behavior, security headers, and publicly referenced endpoints.

For an authorized security assessment, these observations help build an initial profile of the web application. For defenders, the same inspection shows what information is visible to external users and which headers or cookie settings may require improvement.

HTTP headers should be treated as evidence about the server’s public behavior, not as automatic proof of a vulnerability. Any further testing must remain within the approved scope.

---

<div align="center">

**Week 2 | Project Module 1 | Task 5**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorized%20Only-brightgreen?style=flat-square )

</div>
