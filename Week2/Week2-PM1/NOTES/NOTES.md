# 🌐 DNS, Hosting & Server Concepts — Complete Notes

![Internship](https://img.shields.io/badge/Internship-Networkwalks-red)
![Week](https://img.shields.io/badge/Week-2-blue)
![Module](https://img.shields.io/badge/Module-PM1%20Footprinting-orange)
![Type](https://img.shields.io/badge/Type-Concept%20Notes-brightgreen)

> 📚 Concept notes built during the Week 2 Footprinting & Reconnaissance lab, covering DNS, hosting, servers, records, firewalls, and IP addressing.

---

## 📑 Table of Contents

1. [Domain vs Web Server](#1--domain-vs-web-server)
2. [Full DNS Resolution Flow](#2--full-dns-resolution-flow)
3. [Two Types of DNS Servers](#3--two-types-of-dns-servers)
4. [Why There Are Two Different IPs](#4--why-there-are-two-different-ips)
5. [Shared Hosting — One IP, Many Websites](#5--shared-hosting--one-ip-many-websites)
6. [Private IP vs Public IP](#6--private-ip-vs-public-ip)
7. [Whose IP Actually Shows Up?](#7--whose-ip-actually-shows-up)
8. [Network Firewall vs WAF](#8--network-firewall-vs-waf)
9. [Why Websites Must Be On 24/7](#9--why-websites-must-be-on-247)
10. [What Is a DNS Record?](#10--what-is-a-dns-record)
11. [Who Asks For Which Record?](#11--who-asks-for-which-record)
12. [DNS Record Types Explained](#12--dns-record-types-explained)
13. [🎯 Quick Reference Summary Table](#13--quick-reference-summary-table)

---

## 1. 🏷️ Domain vs Web Server

| Term | What it really is |
|---|---|
| 🏷️ **Domain Name** | Just a human-friendly **name** (e.g. `networkwalks.com`). Does **nothing** by itself — no files, no content. |
| 🖥️ **Web Server** | An actual computer (physical/virtual) that **stores website files** (HTML, images, code) and sends them out on request. |
| 📖 **DNS** | The **"phonebook"** that links a domain name → IP address. |

> 💡 **Analogy:**
> - Domain name = a person's **name** ("John Smith")
> - Web server = John's **actual house**
> - DNS = the **phonebook** that tells you John's address

---

## 2. 🔄 Full DNS Resolution Flow

**What happens when you type a domain in the browser:**

| Step | Action |
|---|---|
| 1️⃣ | You type `networkwalks.com` — your computer doesn't know the IP yet |
| 2️⃣ | Browser asks a **DNS Resolver** (e.g. Google `8.8.8.8`) → *"What's the IP of networkwalks.com?"* |
| 3️⃣ | Resolver doesn't know, so it asks the **Authoritative DNS Server** (e.g. `ns6135.hostgator.com`) |
| 4️⃣ | Authoritative server replies: `networkwalks.com = 192.232.216.135` |
| 5️⃣ | Answer is passed back to your browser |
| 6️⃣ | Browser connects **directly** to that IP, sending a `Host:` header too |
| 7️⃣ | Web server reads the Host header, finds the matching site, sends the page back ✅ |

```
🖥️ Browser
   ↓ "What's the IP of networkwalks.com?"
📡 DNS Resolver (e.g. Google 8.8.8.8)
   ↓ asks
🏛️ Authoritative DNS Server (ns6135.hostgator.com → 50.87.144.87)
   ↓ replies "192.232.216.135"
📡 Resolver forwards answer to Browser
   ↓
🖥️ Browser connects to 192.232.216.135 + sends "Host: networkwalks.com"
   ↓
✅ Web Server serves the correct website
```

---

## 3. 🗂️ Two Types of DNS Servers

### 🏛️ A) Authoritative DNS Server
- Holds the **"master record"** for a specific domain
- Example: `ns6135.hostgator.com` / `ns6136.hostgator.com`
- Assigned by the domain owner via their registrar (GoDaddy) to manage DNS
- The **real source of truth** for that domain

### 📡 B) Resolver DNS Server
- The server **your computer/network** is configured to use
- Example: Google's `8.8.8.8`, or your ISP/router's DNS
- Does **NOT** hold the original record — asks the authoritative server and relays/caches the answer

> 💡 **Analogy:**
> - Authoritative DNS Server = the **original property registry office**
> - Resolver = your **local info center** that calls the registry office on your behalf

---

## 4. ⚠️ Why There Are Two Different IPs

> 🔑 **Common confusion — clarified here!**

In `dnsrecon`/`whois` output you'll see **two different IPs**:

| Server | IP |
|---|---|
| 📡 DNS Server (`ns6135.hostgator.com`) | `50.87.144.87` |
| 🖥️ Web Server (`networkwalks.com`) | `192.232.216.135` |

These are different because they're **two separate machines** with **two separate jobs**:
- 📡 **DNS Server** = only tells you *"where the website is"* (like a receptionist/directory service)
- 🖥️ **Web Server** = actually **holds** the website's files (like the shelf where the book physically is)

> 💡 **Analogy (Library):**
> You ask the receptionist (DNS server) *"Where is Harry Potter?"* → Receptionist says *"Shelf 42"* (gives a location, doesn't hold the book itself) → You go to Shelf 42 (Web server) to get the actual book.
>
> The receptionist's desk has its own location (`50.87.144.87`); Shelf 42 has a different location (`192.232.216.135`). **Both real, both different — this is normal and expected.**

**🤔 Why separate machines?** Hosting companies run **dedicated DNS servers** separately from web-hosting servers to:
- ⚡ Keep DNS lookups fast and reliable
- 🛡️ Keep DNS working even if a web server goes down
- ⚖️ Distribute load between specialized machines

---

## 5. 🏢 Shared Hosting — One IP, Many Websites

**❓ Question:** If many websites share the same server (same IP), how does the server know **which** website to show?

**✅ Answer:** The **HTTP Host Header**

When the browser sends a request, it includes an extra line:
```http
GET / HTTP/1.1
Host: networkwalks.com
```

This `Host:` header tells the server **exactly which website's files to serve**, even if hundreds of domains resolve to the same IP.

> 🏷️ This setup is called **"Virtual Hosting"** (name-based hosting) — extremely common with HostGator, GoDaddy, Bluehost, etc.

> 💡 **Analogy (Apartment Building):**
> - IP address = the building's **street address** (same for all)
> - Domain name = a specific **apartment/flat number** inside
> - Host header = telling the guard *"I want flat number 5"* → routed to the correct flat only

🔒 **Bonus — HTTPS/SNI:** For HTTPS, a mechanism called **SNI (Server Name Indication)** does the same job **during the encrypted handshake**, before any data is exchanged — so the server knows which SSL certificate to use.

**👤 Who reads the Host header?** The **web server software** (Apache, Nginx — what `whatweb` detects) checks it and serves the matching site.

---

## 6. 🔐 Private IP vs Public IP

### 🏠 Private IP
- Only valid **inside a local network** (e.g. home WiFi)
- ❌ NOT directly reachable from the internet
- Reserved ranges:
  ```
  10.0.0.0     – 10.255.255.255
  172.16.0.0   – 172.31.255.255
  192.168.0.0  – 192.168.255.255
  ```

### 🌍 Public IP
- ✅ Reachable from **anywhere** on the internet
- Unique across the whole internet
- Required for anything the public needs to access — websites, DNS servers

**Checked in this lab:**

| IP | Type |
|---|---|
| `192.232.216.135` (web server) | 🌍 **Public IP** ✅ |
| `50.87.144.87` (DNS server) | 🌍 **Public IP** ✅ |

> Both **must** be public — random people/computers worldwide need to reach them.

---

## 7. 🕵️ Whose IP Actually Shows Up?

> ⚠️ **Important clarification!**

When you buy hosting (e.g. HostGator) and upload your website files, those files live on **THE HOSTING COMPANY'S SERVER** in their data center — **NOT** on your personal laptop at home.

| The IP in DNS/whois... | Belongs to |
|---|---|
| ✅ | The **hosting company's server** |
| ❌ | NOT the website owner's **personal computer** |

> 💡 **Analogy (Mall):**
> You rent a shop inside a mall (hosting). Your goods (website files) are stored in that shop — the mall's building, not your house. The mall's address is public (customers can visit), but **your home address was never involved**.

**🎯 Who does an attacker actually target?**
If an attacker gets the IP, they target the **hosting server** — **not** the personal device of the website owner. The owner's personal PC is not exposed at all under normal hosting.

> ⚠️ **Shared hosting risk note:** If multiple sites share a server, a **server-level compromise** could affect **all** websites on that server — but it still only affects the **hosting server**, never any individual owner's personal device.

---

## 8. 🛡️ Network Firewall vs WAF

### 🚧 A) Network Firewall (e.g. Cisco ASA, Fortinet, Palo Alto)
- Works at the **network level** (IP addresses, ports, protocols)
- Asks: *"Is this connection allowed on this IP/port?"*
- Protects an **entire network/infrastructure** — servers, employee PCs, internal systems
- Common in enterprise/corporate environments

### 🕸️ B) Web Application Firewall — WAF (e.g. ModSecurity, Cloudflare, Sucuri, Akamai)
- Works at the **application/content level**
- Asks: *"Does this HTTP request contain a malicious pattern (SQLi, XSS)?"*
- Protects a **specific website's traffic** only
- 🔎 This is what `wafw00f` detects in this lab

> 💡 **Analogy (Office Building):**
> - Network Firewall = the **main gate security guard** (checks who/what enters the building at all)
> - WAF = the **receptionist inside one specific office** (checks what you're carrying into *that* office)

🏢 **Real world:** Large orgs often use **both layers together**:
```
🌍 Internet → 🚧 Network Firewall (whole network) → 🕸️ WAF (specific website) → 🖥️ Actual Server
```

---

## 9. ⏰ Why Websites Must Be On 24/7

**❓ Question:** If my website must respond to anyone, anytime — do I need to keep a server running 24/7 myself?

**✅ Answer:** Yes, a server needs to run 24/7 — but **you don't have to manage it yourself.**

This is exactly what **hosting companies** do:
- 🏢 Run large **data centers** — buildings full of servers running continuously
- 🔋 Backup power, cooling, redundant internet connections
- 💳 You **rent space** on their always-on servers
- 🛠️ They keep it running, updated, and secure

> 💡 **Analogy:**
> - Option 1: Build your own shop building — manage your own electricity/security/generator *(self-hosting — hard/expensive)*
> - Option 2: Rent a shop inside a mall — the mall handles everything 24/7 *(using a hosting company — what most people do)*

---

## 10. 📋 What Is a DNS Record?

A **DNS Record** is a **single entry** in DNS's database storing **one specific piece of information** about a domain.

Think of DNS as a big table — each row is a "record":

| Domain | Record Type | Value |
|---|---|---|
| networkwalks.com | 🅰️ A | `192.232.216.135` |
| networkwalks.com | 📧 MX | `mail.networkwalks.com` |
| networkwalks.com | 🏛️ NS | `ns6135.hostgator.com` |
| networkwalks.com | 📝 TXT | `v=spf1 +a +mx ...` |

> 💡 **Analogy:** Records are like lines on a company's **contact card** — phone number, email, address, website — each is a separate "record."

> 🔧 `dnsrecon -d networkwalks.com` basically fetches the domain's **entire contact card** in one go.

---

## 11. 🙋 Who Asks For Which Record?

> ⚠️ DNS does **NOT** decide what a user wants — it's just a **question-answer machine**.

It's the **requesting application** (browser, email app, etc.) that decides what record type it needs and asks DNS specifically for it.

| Requester | Asks for | DNS replies |
|---|---|---|
| 🌐 Browser | A record | `192.232.216.135` |
| 📧 Outlook | SRV/Autodiscover record | `cpanelemaildiscovery.cpanel.net` |

> 💡 **Analogy:** DNS is like a **librarian** who doesn't decide what book you want — **you** tell the librarian "I want a Fiction book," and the librarian just looks it up.

---

## 12. 🔤 DNS Record Types Explained

### 🅰️ A Record (Address)
```
A networkwalks.com 192.232.216.135
```
📌 **Meaning:** The domain's actual **IP address** — where the website files/server live.
👤 **Used by:** Browsers, to know where to connect.

---

### 📧 MX Record (Mail Exchange)
```
MX mail.networkwalks.com 192.232.216.135
```
📌 **Meaning:** Which server handles **incoming email** for this domain.
🗨️ **Simple version:** "This is the mail server's address."

---

### 🏛️ NS Record (Name Server)
```
NS ns6135.hostgator.com 50.87.144.87
NS ns6136.hostgator.com 192.232.216.131
```
📌 **Meaning:** Tells you **who manages the DNS** for this domain — the authoritative server(s).

> 💡 **Analogy:** "This company's HR department / records office is located here" — where to go for any official DNS info.

> ℹ️ Multiple NS records usually exist for **redundancy/backup**.

---

### 📝 TXT Record (Text)

A flexible record type holding **plain text**. Two common uses:

#### 🛡️ SPF (Sender Policy Framework)
```
TXT networkwalks.com v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all
```
📌 **Meaning:** Lists which servers are **authorized to send email** using this domain's name. Anything else may be flagged as spam/spoofed.

> 💡 **Analogy:** An "authorized signature list" — like "only these banks can issue cheques on our behalf."

#### ✅ Site Verification (e.g. Google)
```
TXT networkwalks.com google-site-verification=rr04eRmqHoWY3XemnizDNVK4q75X...
```
📌 **Meaning:** A random code that **proves domain ownership** to a service like Google Search Console.

> 💡 **Analogy:** "Proof of ownership" — write this secret code on your gate, then we'll believe it's your house.

---

### 🔌 SRV Record (Service)
```
SRV _autodiscover._tcp.networkwalks.com cpanelemaildiscovery.cpanel.net 184.94.204.11 443
```
📌 **Meaning:** Which server/port handles a **specific service** for this domain — not the whole domain, one particular function.

**📖 Real scenario — Autodiscover:**
1. 📱 You add `ali@networkwalks.com` to Outlook, typing only email + password
2. 🤔 Outlook doesn't know the mail settings, so it asks DNS: *"What's the autodiscover record?"*
3. 📡 DNS replies: *"Go to `cpanelemaildiscovery.cpanel.net`, port 443"*
4. 🔄 Outlook fetches all technical settings automatically
5. ✅ Account set up — no manual typing needed!

**🤔 Why multiple IPs for the same SRV record?**
You may see 8 different IPs for `cpanelemaildiscovery.cpanel.net`. This is because:
- 🏢 It's a **shared service** run by HostGator/cPanel for **all** their customers
- 🔁 Runs on multiple machines for **redundancy** (backup) and **load balancing**

> 💡 **Analogy:** A courier company's central helpline running from multiple call centers in different cities — if one's busy, others still answer.

| Record | Role |
|---|---|
| 📧 MX | The actual delivery address (where mail physically goes) |
| 🔌 SRV/Autodiscover | The "helpline" that tells your email app *how* to set itself up |

---

### 🏁 SOA Record (Start of Authority)
```
SOA ns6135.hostgator.com 50.87.144.87
```
📌 **Meaning:** Identifies the **primary/master** authoritative DNS server — confirms *"this is the original source of truth"* for the domain.

---

## 13. 🎯 Quick Reference Summary Table

| 🔤 Term | 📖 Meaning |
|---|---|
| 🏷️ Domain Name | Human-friendly website name (no content itself) |
| 🖥️ Web Server | Machine that stores & serves website files |
| 📖 DNS | System that maps domain names → IP addresses |
| 🏛️ Authoritative DNS Server | The "official" server holding a domain's real DNS records |
| 📡 Resolver DNS Server | The server your device queries, which asks the authoritative server for you |
| 🅰️ A Record | Domain → IP address |
| 🏛️ NS Record | Which servers manage this domain's DNS |
| 📧 MX Record | Where email for this domain goes |
| 📝 TXT / SPF Record | Free-form text — email security policy or ownership verification |
| 🔌 SRV Record | Server/port for a **specific service** (e.g. email autodiscover setup) |
| 🏁 SOA Record | Identifies the primary/master DNS server for the domain |
| 🌐 Host Header | Extra info sent with an HTTP request so a shared server knows which website to serve |
| 🏠 Private IP | Only valid inside a local network, not internet-reachable |
| 🌍 Public IP | Reachable from anywhere on the internet |
| 🚧 Network Firewall | Protects an entire network (IP/port level) — e.g. Cisco |
| 🕸️ WAF (Web App Firewall) | Protects a specific website's traffic (content level) — e.g. ModSecurity |

---

<div align="center">

**📚 Prepared for:** Networkwalks Academy — Week 2, Project Module 1 (Footprinting & Reconnaissance)

![Educational](https://img.shields.io/badge/Purpose-Educational%20Only-yellow)
![Legal](https://img.shields.io/badge/Authorized-networkwalks.com-brightgreen)

</div>
