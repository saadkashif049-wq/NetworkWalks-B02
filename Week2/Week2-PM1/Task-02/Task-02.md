# 🌐 Task 2 — WhatWeb Technology Fingerprinting

![Tool](https://img.shields.io/badge/Tool-whatweb-orange)
![Category](https://img.shields.io/badge/Category-Passive%20Recon-blue)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-informational)
![Legality](https://img.shields.io/badge/Legality-Fully%20Legal-brightgreen)
![Target%20Risk](https://img.shields.io/badge/Target%20Risk-None%20(Read--Only)-yellow)
![IP%20Exposure](https://img.shields.io/badge/Your%20IP%20Exposed-No-success)

> 📚 **Lab:** Networkwalks Internship — Week 2, Project Module 1 (Footprinting & Reconnaissance)
> 🎯 **Target:** `networkwalks.com` (authorized training domain)

---

## 📑 Table of Contents

1. [What Is WhatWeb?](#1--what-is-whatweb)
2. [How It Works — The Mechanism](#2--how-it-works--the-mechanism)
3. [Command Syntax](#3--command-syntax)
4. [Step-by-Step: Running the Task](#4--step-by-step-running-the-task)
5. [Why Multiple Lines of Output Appear](#5--why-multiple-lines-of-output-appear)
6. [Reading the Output — Field by Field](#6--reading-the-output--field-by-field)
7. [🔴 Attacker Perspective](#7--attacker-perspective)
8. [🔵 Defender Perspective](#8--defender-perspective)
9. [📝 Report Checklist](#9--report-checklist)

---

## 1. 🧭 What Is WhatWeb?

**WhatWeb** fingerprints the **technology stack** running behind a live website — the web server software, CMS, plugins, frameworks, and more — just by analyzing what the site publicly reveals.

Unlike WHOIS (which looks at domain *registration*), WhatWeb looks at the **actual running website itself**.

---

## 2. ⚙️ How It Works — The Mechanism

```
🖥️ whatweb sends a normal HTTP request
      ↓ (exactly like a browser would)
🌐 Target website responds
      ↓
🔍 whatweb analyzes:
      • HTML source code patterns
      • HTTP response headers
      • Cookie names
      • Meta tags
      • JavaScript library signatures
      ↓
📚 Matches against a huge signature database
      ↓
✅ Prints identified CMS, plugins, server software, versions
```

WhatWeb maintains a database of **thousands of known "signatures"** for CMSs, plugins, and frameworks — similar to how antivirus software matches files against known virus signatures.

---

## 3. 💻 Command Syntax

```bash
whatweb <domain-name>
```

**Example used in this lab:**
```bash
whatweb networkwalks.com
```

**✅ IP Exposure Check:** Your own IP is **never** revealed — WhatWeb only sends an outgoing request to the target and reports on what the target reveals about itself.

---

## 4. 🪜 Step-by-Step: Running the Task

| Step | Action |
|---|---|
| 1️⃣ | Open a terminal in Kali Linux |
| 2️⃣ | Run: `whatweb networkwalks.com` |
| 3️⃣ | Wait for the output to print |
| 4️⃣ | 📸 Take a screenshot of the terminal |
| 5️⃣ | 💾 Save the output to a text file: `whatweb networkwalks.com > task2-whatweb.txt` |

---

## 5. 🧩 Why Multiple Lines of Output Appear

WhatWeb often shows **3 separate results** for one domain — because it follows redirects:

```
1️⃣ http://networkwalks.com    → [301 Moved Permanently] → redirecting...
2️⃣ https://networkwalks.com   → [200 OK] → final destination reached
3️⃣ https://networkwalks.com/  → [200 OK] → same page, trailing slash
```

📌 The **301 redirect** from `http` → `https` is actually a **good security sign** — the site is forcing secure (encrypted) connections.

---

## 6. 📋 Reading the Output — Field by Field

### 🖥️ Server Info
```
Apache, HTTPServer[Apache]
```
➡️ Web server software: **Apache**

### 📍 Network Info
```
IP[192.232.216.135]
```
➡️ Server's IP address (matches `nslookup` results — consistent)

### 🎯 CMS/Platform Info — Most Important for Security
```
MetaGenerator[WordPress 7.0.4, WordPress Download Manager 3.3.58]
WordPress[7.0.4]
```
➡️ **CMS:** WordPress version `7.0.4`
➡️ **Plugin:** WordPress Download Manager version `3.3.58`

> 🚨 This is the highest-value info for an attacker — exact versions can be checked against vulnerability databases (CVE, WPScan, exploit-db).

### 🎨 Frontend Libraries
```
Bootstrap[7.0.4], JQuery[3.7.1], HTML5
```
➡️ Frameworks used for design/frontend

### 📧 Contact/Leak Info
```
Email[info@networkwalks.com]
```
➡️ A public email exposed (found somewhere in the source code)

### 🍪 Cookies
```
Cookies[__wpdm_client], HttpOnly[__wpdm_client]
```
➡️ Cookie name `__wpdm_client` set (related to WP Download Manager plugin)
➡️ `HttpOnly` flag present — 🔒 **good security practice** (cookie can't be accessed via JavaScript, reducing XSS risk)

### 📊 Tracking/Analytics
```
Google-Tag-Manager
```
➡️ Site uses Google Tag Manager for analytics/tracking

### 📄 Page Metadata
```
Title[Networkwalks Academy]
Open-Graph-Protocol[website]
```
➡️ Page title + social media preview tags

### 🧱 Uncommon Headers
```
UncommonHeaders[permissions-policy,link,upgrade,referrer-policy,x-endurance-cache-level,x-nginx-cache]
```
➡️ `x-endurance-cache-level` and `x-nginx-cache` suggest a **caching layer/CDN** is involved (likely tied to HostGator's parent infrastructure)

---

## 7. 🔴 Attacker Perspective

| Field Found | How Attacker Uses It |
|---|---|
| `WordPress[7.0.4]` | Searches exploit-db/WPScan for known CVEs in this exact version |
| `WordPress Download Manager 3.3.58` | 🚨 Most dangerous — plugins are often less secure than core; searches for known plugin exploits |
| `IP[192.232.216.135]` | Uses for direct port scanning, reverse-IP lookups |
| `Email[info@networkwalks.com]` | Target for phishing/social engineering |
| `Bootstrap`, `JQuery` versions | Checks for outdated frontend library vulnerabilities (e.g. XSS) |

> 🧠 **Attacker's mindset:** *"I now know the exact software stack — I don't need to guess, I can directly target known weaknesses."*

---

## 8. 🔵 Defender Perspective

This is the exact same tool defenders run on their **own** sites for **self-recon / attack surface assessment**:

| Action | Why |
|---|---|
| ✅ Check version leakage | Decide whether to hide WordPress version via security plugins |
| ✅ Identify outdated software | Patch/update before an attacker exploits it |
| ✅ Reduce unnecessary exposure | Hide sensitive info like exposed emails if not needed |
| ✅ Audit plugin inventory | Remove unused/outdated plugins to shrink attack surface |

> 🧠 **Defender's mindset:** *"If I can find this without logging in, so can an attacker — let's fix it first."*

---

## 9. 📝 Report Checklist

- [ ] Command used documented: `whatweb networkwalks.com`
- [ ] Screenshot saved
- [ ] Output saved to `.txt` file
- [ ] Web server software identified (Apache)
- [ ] CMS + version identified (WordPress 7.0.4)
- [ ] Plugin + version identified (WP Download Manager 3.3.58)
- [ ] Frontend frameworks noted (Bootstrap, JQuery)
- [ ] Exposed email noted
- [ ] HTTPS redirect confirmed (good practice)
- [ ] Cookie security flags noted (HttpOnly)

---

<div align="center">

**📚 Task 2 of 6 — Footprinting & Reconnaissance Lab**
**Previous:** Task 1 — WHOIS | **Next:** Task 3 — nslookup

![Educational](https://img.shields.io/badge/Purpose-Educational%20Only-yellow)
![Authorized](https://img.shields.io/badge/Authorized%20Target-networkwalks.com-brightgreen)

</div>
