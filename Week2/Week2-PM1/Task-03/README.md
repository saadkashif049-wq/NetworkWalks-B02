# Task 3 — DNS Resolution with `nslookup`

![Week](https://img.shields.io/badge/Week-02-blue?style=flat-square )
![Module](https://img.shields.io/badge/Module-PM1%20%7C%20Footprinting-orange?style=flat-square )
![Task](https://img.shields.io/badge/Task-03%20%7C%20nslookup-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square )
![Purpose](https://img.shields.io/badge/Purpose-Educational%20Only-yellow?style=flat-square )

> **Project:** Week 2 — Project Module 1: Footprinting and Reconnaissance
>
> **Objective:** Resolve `networkwalks.com` to its IP address using DNS and interpret the result from attacker and defender perspectives.
>
> **Authorization:** Perform reconnaissance only against systems you own or have explicit permission to assess.

---

## Table of Contents

- [What Is `nslookup`?](#what-is-nslookup)
- [How It Works — The Mechanism](#how-it-works--the-mechanism)
- [Command Syntax](#command-syntax)
- [Step-by-Step: Running the Task](#step-by-step-running-the-task)
- [Reading the Output — Line by Line](#reading-the-output--line-by-line)
- [Two Types of DNS Servers Involved](#two-types-of-dns-servers-involved)
- [Attacker Perspective](#-attacker-perspective)
- [Defender Perspective](#-defender-perspective)
- [Report Checklist](#-report-checklist)
- [Conclusion](#conclusion)
- [References](#references)

---

## What Is `nslookup`?

`nslookup`, short for **Name Server Lookup**, is a command-line utility used to query the Domain Name System (DNS). It can resolve domain names to IP addresses, perform reverse lookups, identify mail servers, query name servers, and request specific DNS record types.

For this task, `nslookup` answers the following question:

> **Which IP address is associated with `networkwalks.com`?**

DNS associates domain names with information such as IPv4 addresses, IPv6 addresses, mail servers, and authoritative name servers.[1] [2]

| Domain | Observed IPv4 Address | Meaning |
|---|---|---|
| `networkwalks.com` | `192.232.216.135` | IPv4 address observed in the laboratory output |

> **Note:** DNS results can change because of hosting migrations, load balancing, CDNs, proxies, failover systems, or DNS updates. Treat the address as a time-specific observation.

---

## How It Works — The Mechanism

When the following command is executed:

```bash
nslookup networkwalks.com
```

`nslookup` sends a DNS query to the resolver configured on the Kali Linux system. The resolver may be provided by a router, ISP, VPN, enterprise network, or public DNS provider.

### DNS Resolution Flow

```text
Kali Linux terminal
        |
        |  nslookup networkwalks.com
        v
Configured recursive DNS resolver
        |
        |  Checks cache or queries DNS hierarchy
        v
Authoritative DNS server
        |
        |  Returns the DNS record
        v
Recursive resolver returns the answer
        |
        v
nslookup displays the result
```

The normal process is:

1. `nslookup` reads the supplied domain name.
2. It identifies the configured DNS resolver.
3. It requests the default address record, usually an `A` record for IPv4.
4. The resolver checks its cache or performs upstream DNS queries.
5. The authoritative DNS infrastructure returns the record.
6. `nslookup` displays the resolver, domain name, and returned address.

The command normally queries DNS infrastructure rather than connecting to or downloading content from the web server. DNS queries are still network activity and must remain within the authorized assessment scope.

---

## Command Syntax

### Basic Syntax

```text
nslookup [options] [domain-or-IP] [dns-server]
```

Required command:

```bash
nslookup networkwalks.com
```

Because no DNS server is specified, the command uses the system's default resolver.

### Useful Variations

| Command | Purpose |
|---|---|
| `nslookup networkwalks.com` | Query the default resolver. |
| `nslookup networkwalks.com 8.8.8.8` | Query Google Public DNS explicitly. |
| `nslookup -type=A networkwalks.com` | Request an IPv4 address record. |
| `nslookup -type=AAAA networkwalks.com` | Request an IPv6 address record. |
| `nslookup -type=NS networkwalks.com` | Request authoritative name-server records. |
| `nslookup -type=MX networkwalks.com` | Request mail-exchange records. |
| `nslookup 192.232.216.135` | Attempt a reverse DNS lookup. |
| `nslookup -debug networkwalks.com` | Display additional diagnostic information. |

### Interactive Mode

```bash
nslookup
```

Example interactive session:

```text
> server 8.8.8.8
> set type=A
> networkwalks.com
> exit
```

---

## Step-by-Step: Running the Task

### 1. Open the Terminal

Launch a terminal in Kali Linux.

### 2. Confirm That `nslookup` Is Available

```bash
which nslookup
```

### 3. Run the Required Command

```bash
nslookup networkwalks.com
```

### 4. Save the Output

```bash
nslookup networkwalks.com | tee task-3-nslookup.txt
```

### 5. Capture Evidence

Take a screenshot showing the terminal, executed command, complete output, and timestamp or terminal prompt where possible.

### 6. Record the Result

The attached laboratory material reports the following observed IPv4 address:

```text
192.232.216.135
```

Always record the result returned by your own execution because DNS responses may change between resolvers and over time.

### 7. Organize the Evidence

```text
Task-3-nslookup/
├── README.md
├── task-3-nslookup.txt
└── screenshots/
    └── nslookup-networkwalks.png
```

---

## Reading the Output — Line by Line

A typical response may look similar to this:

```text
Server:         192.168.1.1
Address:        192.168.1.1#53

Non-authoritative answer:
Name:           networkwalks.com
Address:        192.232.216.135
```

| Output | Interpretation |
|---|---|
| `Server: 192.168.1.1` | The DNS resolver that answered the request. |
| `Address: 192.168.1.1#53` | The resolver address and DNS service port `53`. |
| `Non-authoritative answer:` | The response came from a recursive resolver or cache rather than the authoritative source. |
| `Name: networkwalks.com` | The queried domain name. |
| `Address: 192.232.216.135` | The IPv4 address returned by DNS. |

### Meaning of “Non-authoritative”

A non-authoritative response does not necessarily mean that the answer is incorrect. It means that the queried server is acting as a recursive resolver or cache instead of being the authoritative DNS server for the domain.

To identify authoritative name servers, query the `NS` record:

```bash
nslookup -type=NS networkwalks.com
```

To compare the result with Google Public DNS:

```bash
nslookup networkwalks.com 8.8.8.8
```

### Common Errors

| Error | Possible Explanation |
|---|---|
| `connection timed out` | The resolver did not respond within the configured time. |
| `no servers could be reached` | No usable DNS resolver was reachable. |
| `NXDOMAIN` | The queried domain does not exist in the current DNS view. |
| `SERVFAIL` | The resolver encountered a processing or validation failure. |
| `REFUSED` | The DNS server refused the query according to its policy. |
| No address returned | The record may not exist or may point to another record such as `CNAME`. |

---

## Two Types of DNS Servers Involved

The DNS server shown in the output is not necessarily the server that owns the official DNS record.[2]

### 1. Recursive Resolver

A **recursive resolver** is the DNS service contacted by Kali Linux. It checks its cache and obtains answers from other DNS servers when necessary.

Examples include:

- A home or office router.
- An ISP resolver.
- Google Public DNS at `8.8.8.8`.
- Cloudflare DNS at `1.1.1.1`.
- An enterprise or VPN security resolver.

### 2. Authoritative DNS Server

An **authoritative DNS server** publishes the official records for a domain or DNS zone. These records may include `A`, `AAAA`, `NS`, `MX`, `TXT`, and `SOA` records.

| Server Role | Responsibility | Visible in Basic Output? |
|---|---|---|
| Recursive resolver | Finds, caches, and returns DNS answers | Usually yes, shown as `Server` |
| Authoritative server | Publishes official DNS records | Not necessarily; query `NS` records to identify it |

---

## 🔴 Attacker Perspective

From an attacker’s perspective, DNS resolution is an early infrastructure-mapping step. During an **authorized security assessment**, the returned address can help an analyst understand where public traffic for a domain is directed.

A permitted assessment may use the result to:

- Record the domain-to-IP relationship.
- Compare responses from multiple DNS resolvers.
- Identify possible shared hosting, reverse proxies, or CDNs.
- Determine whether multiple IPv4 or IPv6 addresses are returned.
- Correlate the address with other approved reconnaissance findings.
- Plan only those additional checks permitted by the rules of engagement.

The observed address from the laboratory material is `192.232.216.135`. It should be treated as a time-specific lab observation and validated before being used in a technical conclusion.

> **Safety boundary:** Discovering an IP address does not authorize port scanning, exploitation, authentication attempts, or testing of unrelated domains that share the same infrastructure.

---

## 🔵 Defender Perspective

Defenders can perform the same lookup against their own domains to understand what information is publicly exposed through DNS.

A defensive review should consider:

- Whether DNS records point to the intended hosting provider.
- Whether an origin server IP is unintentionally exposed behind a CDN or reverse proxy.
- Whether obsolete, test, or staging records remain public.
- Whether multiple records create unexpected routing or exposure.
- Whether DNS changes are monitored and reviewed.
- Whether authoritative name servers are redundant and correctly configured.
- Whether records reveal unnecessary service or infrastructure details.

| Finding | Recommended Defensive Action |
|---|---|
| Unexpected public IP | Confirm ownership and remove stale records where appropriate. |
| Origin IP exposed behind a proxy | Restrict direct origin access and permit traffic only from approved proxy or CDN ranges where practical. |
| Stale subdomain or test record | Remove it or protect the associated service with authentication and network controls. |
| Excessive TXT or service records | Review whether each record is still required. |
| Inconsistent resolver results | Check TTLs, propagation, delegation, and authoritative zone configuration. |
| No DNS monitoring | Enable registrar, DNS-provider, and change-management alerts. |

> **Defensive goal:** Reduce unnecessary public information while preserving the DNS records required for legitimate web, email, and service discovery.

---


---

## Conclusion

The `nslookup` command provides a simple way to connect a human-readable domain name with DNS-published network information. In this task, `networkwalks.com` resolved to the observed IPv4 address `192.232.216.135`.

The resolver shown in the output and the authoritative DNS server may be different systems. The resolver answers the client’s query, while the authoritative server publishes the official DNS data for the domain.

DNS resolution is a useful initial reconnaissance and defensive inventory step, but results must be interpreted carefully because caching, hosting changes, proxies, shared infrastructure, and DNS configuration can affect the answer.

---

## References

[1]: https://datatracker.ietf.org/doc/html/rfc1035 "RFC 1035 — Domain Names: Implementation and Specification"
[2]: https://datatracker.ietf.org/doc/html/rfc9499 "RFC 9499 — DNS Terminology"
[3]: https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/nslookup "Microsoft Learn — nslookup command"
[4]: https://bind9.readthedocs.io/en/stable/manpages.html "BIND 9 Documentation — Manual Pages"

---

<div align="center">

**Week 2 | Project Module 1 | Task 3**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorized%20Only-brightgreen?style=flat-square )

</div>

> **Final note:** Rerun the command and record the live result before submitting a current report.

---

## GitHub Push Commands

```bash
git add README.md task-3-nslookup.txt screenshots/
git commit -m "docs: add Task 3 nslookup reconnaissance report"
git push origin main
```
