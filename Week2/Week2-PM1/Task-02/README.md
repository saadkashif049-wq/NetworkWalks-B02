# Task 2 — Web Technology Fingerprinting with `whatweb`

![Week](https://img.shields.io/badge/Week-02-blue?style=flat-square )
![Module](https://img.shields.io/badge/Module-PM1%20%7C%20Footprinting-orange?style=flat-square )
![Task](https://img.shields.io/badge/Task-02%20%7C%20whatweb-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square )
![Purpose](https://img.shields.io/badge/Purpose-Educational%20Only-yellow?style=flat-square )

> **Project:** Week 2 — Project Module 1: Footprinting and Reconnaissance
>
> **Objective:** Identify the web server, CMS, plugins, frameworks, technologies, and other publicly visible details used by `networkwalks.com`.
>
> **Authorization:** Perform reconnaissance only against websites and systems you own or have explicit permission to assess.

---

## Table of Contents

- [What Is `whatweb`?](#what-is-whatweb)
- [How It Works](#how-it-works)
- [Command Syntax](#command-syntax)
- [Step-by-Step: Running the Task](#step-by-step-running-the-task)
- [Reading the Output](#reading-the-output)
- [Technology Categories](#technology-categories)
- [🔴 Attacker Perspective](#-attacker-perspective)
- [🔵 Defender Perspective](#-defender-perspective)
- [Conclusion](#conclusion)

---

## What Is `whatweb`?

`whatweb` is a web-technology fingerprinting tool included with Kali Linux. It examines a website and attempts to identify the technologies that are publicly exposed by the target.

Depending on the target and its configuration, `whatweb` may identify:

- Web-server software.
- Content-management systems.
- CMS plugins and components.
- JavaScript frameworks and libraries.
- Web technologies and HTTP headers.
- Cookies and security-related information.
- Server IP addresses.
- Email addresses or other visible metadata.
- Technology versions, when they are exposed.

For this task, the target is:

```text
networkwalks.com
```

The goal is to create an initial technology profile without attempting to exploit the detected software.

> **Important:** Fingerprinting results are indicators, not absolute proof. A technology may be hidden, modified, proxied, outdated, or incorrectly identified.

---

## How It Works

When the following command is executed:

```bash
whatweb networkwalks.com
```

`whatweb` sends web requests to the target and analyzes the responses. It compares visible response characteristics against fingerprint patterns known as plugins.

### Fingerprinting Flow

```text
Kali Linux terminal
        |
        |  whatweb networkwalks.com
        v
Target website
        |
        |  Returns HTML, headers, cookies, and other public information
        v
WhatWeb fingerprinting plugins analyze the response
        |
        v
Detected technologies and metadata are displayed
```

The tool may analyze information such as:

1. HTTP response headers.
2. HTML page content.
3. Meta tags and generator fields.
4. Cookies and naming patterns.
5. JavaScript files and paths.
6. Common CMS paths.
7. Server banners and response behavior.

Unlike purely passive information gathering, `whatweb` normally sends requests to the target website. Therefore, it should be used only within an authorized scope.

---

## Command Syntax

### Basic Syntax

```text
whatweb [options] target
```

Required command:

```bash
whatweb networkwalks.com
```

### Useful Variations

| Command | Purpose |
|---|---|
| `whatweb networkwalks.com` | Perform a standard technology scan. |
| `whatweb -a 1 networkwalks.com` | Use a low-aggression scan. |
| `whatweb -a 3 networkwalks.com` | Use a more detailed scan. |
| `whatweb -v networkwalks.com` | Display verbose output. |
| `whatweb --log-verbose=whatweb-report.txt networkwalks.com` | Save verbose results to a file. |
| `whatweb --color=never networkwalks.com` | Disable terminal colors for cleaner saved output. |
| `whatweb -h` | Display the help menu. |

### Recommended Evidence Command

The following command displays the results and saves a clean copy:

```bash
whatweb --color=never networkwalks.com | tee task-2-whatweb.txt
```

---

## Step-by-Step: Running the Task

### 1. Open the Terminal

Launch a terminal in Kali Linux.

### 2. Confirm That `whatweb` Is Available

```bash
which whatweb
```

### 3. Run the Required Command

```bash
whatweb networkwalks.com
```

### 4. Save the Output

```bash
whatweb --color=never networkwalks.com | tee task-2-whatweb.txt
```

### 5. Capture Evidence

Take a screenshot showing:

- The Kali Linux terminal.
- The command that was executed.
- The complete `whatweb` output.
- The terminal prompt or timestamp, if available.

### 6. Organize the Evidence

```text
Task-2-whatweb/
├── README.md
├── task-2-whatweb.txt
└── screenshots/
    └── whatweb-networkwalks.png
```

---

## Reading the Output

A typical `whatweb` result may contain several technology indicators on one line. The exact output depends on the website, network conditions, scan-aggression level, and information exposed by the server.

Example structure:

```text
networkwalks.com [200 OK]
HTTPServer[Apache]
IP[192.232.216.135]
WordPress
Plugin[WP Download Manager]
Email[admin@example.com]
```

| Output Element | Meaning |
|---|---|
| `200 OK` | The server successfully returned the requested resource. |
| `HTTPServer[Apache]` | The response appears to identify Apache as the web server. |
| `IP[192.232.216.135]` | The IP address associated with the observed response. |
| `WordPress` | The site appears to use the WordPress content-management system. |
| `Plugin[...]` | A plugin or component may have been identified from public page information. |
| `Email[...]` | An email address may have been detected in publicly visible content. |
| Version number | A possible software version exposed by the website. |

> **Interpretation rule:** A detected technology should be verified before making a security conclusion. Fingerprinting tools can produce false positives or identify a component that is no longer active.

---

## Technology Categories

### Web Server

The web-server result may identify software such as Apache, Nginx, Microsoft IIS, or another HTTP server. Server banners can help defenders understand what information is publicly exposed.

### Content-Management System

`whatweb` may identify platforms such as WordPress, Joomla, Drupal, or other CMS applications. CMS detection often comes from HTML markers, generator tags, known paths, scripts, or cookies.

### Plugins and Components

Publicly visible plugin names and versions can reveal additional components installed on a CMS. The attached task material identifies WordPress and a WP Download Manager component in the example observations.

### Frameworks and Libraries

The tool may detect client-side libraries, JavaScript frameworks, analytics services, CSS frameworks, or other application components.

### IP Address

The output may show the IP address associated with the website, such as:

```text
192.232.216.135
```

The result should be treated as a time-specific observation because websites may use CDNs, reverse proxies, multiple addresses, or shared hosting.

### Email Addresses and Metadata

Email addresses or other metadata may be extracted from visible page content. This information should be handled responsibly and must not be used for spam, phishing, social engineering, or unauthorized contact.

---

## 🔴 Attacker Perspective

From an attacker’s perspective, `whatweb` is useful for building a technology profile before attempting any further activity. Detected software and versions may be compared with publicly known vulnerability information during an authorized assessment.

A permitted assessment may use the results to:

- Identify the apparent web-server platform.
- Identify the CMS and installed components.
- Record software versions exposed by the target.
- Compare identified versions with approved vulnerability databases.
- Detect potentially interesting paths, plugins, or public metadata.
- Plan only the next checks allowed by the rules of engagement.

The task material highlights that a WordPress installation, plugin information, server details, IP address, or email address may be exposed through fingerprinting.

> **Safety boundary:** Detecting a technology or version does not prove that the system is vulnerable. Do not exploit, brute-force, scan, or attack any detected component without explicit authorization.

---

## 🔵 Defender Perspective

Defenders should run technology-fingerprinting checks against their own websites to understand what information is publicly visible to external observers.

A defensive review should consider:

- Whether the web-server banner should be hidden or minimized.
- Whether CMS and plugin versions are publicly exposed.
- Whether unused plugins, themes, frameworks, or libraries remain installed.
- Whether email addresses or sensitive metadata appear in page content.
- Whether old JavaScript libraries contain known security weaknesses.
- Whether HTTP security headers are configured correctly.
- Whether the detected software is patched and supported.

| Finding | Recommended Defensive Action |
|---|---|
| Web-server version exposed | Minimize unnecessary server-banner information. |
| Outdated CMS or plugin | Patch, upgrade, replace, or remove the component. |
| Unused plugin or theme | Remove it rather than leaving it disabled. |
| Public email address | Use appropriate privacy and anti-abuse controls. |
| Sensitive metadata exposed | Review page source, headers, comments, and public files. |
| Outdated JavaScript library | Upgrade the library and remove unused dependencies. |
| Weak HTTP security configuration | Review security headers, cookies, TLS, and redirect behavior. |

> **Defensive goal:** Reduce unnecessary technology disclosure while keeping the website patched, functional, and maintainable.

---

## Conclusion

The `whatweb` command provides a fast way to identify technologies that may be publicly exposed by a website. It can reveal the apparent web server, CMS, plugins, frameworks, IP address, version information, and other metadata.

For an authorized security assessment, these findings help create an initial technology inventory. For defenders, the same results show what an external observer can learn and which components may require stronger configuration, patching, or information-disclosure controls.

Fingerprinting results should always be validated before being treated as confirmed technical facts. The presence of a detected technology does not automatically mean that it is vulnerable.

---

<div align="center">

**Week 2 | Project Module 1 | Task 2**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorized%20Only-brightgreen?style=flat-square )

</div>

> **Final note:** Run the command, save the complete output, capture the screenshot, and record the current result before submitting the task.

---

## GitHub Push Commands

```bash
git add README.md task-2-whatweb.txt screenshots/
git commit -m "docs: add Task 2 whatweb reconnaissance report"
git push origin main
```
