# 🔍 Task 1 — WHOIS Domain Reconnaissance

![Tool](https://img.shields.io/badge/Tool-whois-orange)
![Category](https://img.shields.io/badge/Category-Passive%20Recon-blue)
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-informational)
![Legality](https://img.shields.io/badge/Legality-Fully%20Legal-brightgreen)
![Target%20Risk](https://img.shields.io/badge/Target%20Risk-None%20(Read--Only)-yellow)
![IP%20Exposure](https://img.shields.io/badge/Your%20IP%20Exposed-No-success)

> 📚 **Lab:** Networkwalks Internship — Week 2, Project Module 1 (Footprinting & Reconnaissance)
> 🎯 **Target:** `networkwalks.com` (authorized training domain)

---

## 📑 Table of Contents

1. [What Is WHOIS?](#1--what-is-whois)
2. [How It Works — The Mechanism](#2--how-it-works--the-mechanism)
3. [Command Syntax](#3--command-syntax)
4. [Step-by-Step: Running the Task](#4--step-by-step-running-the-task)
5. [Reading the Output — Line by Line](#5--reading-the-output--line-by-line)
6. [Why Two Blocks of Output Appear](#6--why-two-blocks-of-output-appear)
7. [WHOIS Privacy Protection](#7--whois-privacy-protection)
8. [🔴 Attacker Perspective](#8--attacker-perspective)
9. [🔵 Defender Perspective](#9--defender-perspective)
10. [📝 Report Checklist](#10--report-checklist)

---

## 1. 🧭 What Is WHOIS?

**WHOIS** (pronounced *"who is"*) is a query protocol used to look up the **public registration record** of a domain name. It answers: *"Who registered this domain, and when?"*

Every domain must be registered through an accredited **registrar** (GoDaddy, Namecheap, etc.), and that registration info is stored in a **publicly queryable database**. The `whois` command sends a request to that database and prints the result.

---

## 2. ⚙️ How It Works — The Mechanism

```
🖥️ You (whois command)
      ↓
🏛️ Registry (e.g. Verisign for .com domains)
      ↓ "who's the registrar for this domain?"
🏢 Registrar (e.g. GoDaddy)
      ↓
📋 Full registration record returned
```

1. `whois` first contacts the **domain registry** — the organization managing an entire TLD (e.g. `.com` is run by Verisign)
2. The registry replies with basic info and points to the **registrar** handling this specific domain
3. `whois` then queries the **registrar's own WHOIS server** for the full, detailed record
4. This is why output sometimes shows **two blocks** — a short one from the registry, a fuller one from the registrar

---

## 3. 💻 Command Syntax

```bash
whois <domain-name>
```

**Example used in this lab:**
```bash
whois networkwalks.com
```

**✅ IP Exposure Check:** Your own IP is **never** revealed by this command — it only queries a public registry/registrar database, not the target server itself.

---

## 4. 🪜 Step-by-Step: Running the Task

| Step | Action |
|---|---|
| 1️⃣ | Open a terminal in Kali Linux |
| 2️⃣ | Run: `whois networkwalks.com` |
| 3️⃣ | Wait for the output to print (usually instant) |
| 4️⃣ | 📸 Take a screenshot of the terminal |
| 5️⃣ | 💾 Save the output to a text file: `whois networkwalks.com > task1-whois.txt` |

---

## 5. 📋 Reading the Output — Line by Line

| Field | Meaning |
|---|---|
| `Domain Name` | The domain being queried |
| `Registry Domain ID` | Unique internal ID the registry (Verisign) uses to track this domain |
| `Registrar` | Company the domain was purchased through (e.g. **GoDaddy**) |
| `Registrar WHOIS Server` | Which server holds the full/detailed record |
| `Creation Date` | When the domain was first registered |
| `Updated Date` | Last time the record was modified |
| `Registry Expiry Date` | When the registration needs renewal |
| `Registrar Abuse Contact` | Email/phone to report abuse — belongs to the **registrar**, not the site owner |
| `Domain Status` (×4) | Security locks protecting the domain from unauthorized delete/renew/transfer/update — 🔒 **normal & good practice** |
| `Name Server` (×2 or more) | 🎯 **Most valuable line for recon** — reveals the **hosting provider** (e.g. HostGator) |
| `DNSSEC` | Whether DNS records are cryptographically signed (`signed`/`unsigned`) |
| `Registrant Name/Org/Address` | Owner's details — **often hidden** behind privacy protection |

### 🎯 Key finding in this lab
```
Name Server: NS6135.HOSTGATOR.COM
Name Server: NS6136.HOSTGATOR.COM
```
➡️ Even though **GoDaddy** is the registrar (where the domain was bought), these name servers reveal the site is actually **hosted on HostGator**.

> 💡 **Remember:** Registrar ≠ Host
> - **Registrar** = where you bought the domain name
> - **Host** = where the actual website files/server live

---

## 6. 🧩 Why Two Blocks of Output Appear

Your real output likely showed **two similar-looking blocks**:

1. **First block** — from the **Registry** (Verisign) — a "thin" response, mostly pointing to the registrar
2. **Second block** — from the **Registrar's own server** (`whois.godaddy.com`) — the "thick" response with full details, including registrant contact info

This happens because `whois` automatically re-queries the registrar after the registry tells it where to look.

---

## 7. 🕵️ WHOIS Privacy Protection

Most domain owners today use **WHOIS privacy protection** — a free service (often from GoDaddy) that hides real personal info:

```
Registrant Name: Registration Private
Registrant Organization: Domains By Proxy, LLC
Registrant Email: https://www.godaddy.com/whois/results.aspx?...
```

📌 Instead of the real owner's name/address/email, you see the **proxy service's info**, and any contact happens through a masked web form.

> ℹ️ You'll also sometimes see a notice like:
> ```
> **NOTICE** This WHOIS server is being retired. Please use our RDAP service instead.
> ```
> This just means the registrar is moving to **RDAP** (Registration Data Access Protocol) — the modern JSON-based replacement for WHOIS.

---

## 8. 🔴 Attacker Perspective

**How an attacker uses this output:**

| Info Gained | How It's Used |
|---|---|
| 🏢 Hosting provider (via NS records) | Narrows down IP ranges to scan; identifies the hosting platform for potential social engineering |
| 📅 Registration/expiry dates | Can help plan domain-related social engineering attempts |
| 📧 Registrar abuse contact | Understanding communication channels around the domain |
| 🔒 Domain status locks | Confirms whether the domain has anti-hijacking protections |

> ⚠️ None of this is an "attack" — it's purely **passive, publicly available information**, which is exactly why footprinting is powerful and hard to detect.

---

## 9. 🔵 Defender Perspective

**Why organizations run WHOIS on themselves:**

- ✅ Confirm what's **publicly exposed** about their domain
- ✅ Verify **privacy protection** is active (if desired)
- ✅ Confirm **security locks** (transfer/delete-prohibited) are enabled
- ✅ Track domain **expiry dates** to avoid accidental lapse (a common attack vector — expired domains can be re-registered by attackers!)

---

## 10. 📝 Report Checklist

- [ ] Command used documented: `whois networkwalks.com`
- [ ] Screenshot saved
- [ ] Output saved to `.txt` file
- [ ] Registrar identified (GoDaddy)
- [ ] Hosting provider identified via name servers (HostGator)
- [ ] Registration/expiry dates noted
- [ ] Privacy protection status noted (Domains By Proxy)
- [ ] DNSSEC status noted (unsigned)
- [ ] Domain status locks noted

---

<div align="center">

**📚 Task 1 of 6 — Footprinting & Reconnaissance Lab**
**Next:** Task 2 — WhatWeb (Technology Fingerprinting)

![Educational](https://img.shields.io/badge/Purpose-Educational%20Only-yellow)
![Authorized](https://img.shields.io/badge/Authorized%20Target-networkwalks.com-brightgreen)

</div>
