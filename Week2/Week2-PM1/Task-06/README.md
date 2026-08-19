# Task 6 — DNS Record Enumeration with `dnsrecon`

![Week](https://img.shields.io/badge/Week-02-blue?style=flat-square )
![Module](https://img.shields.io/badge/Module-PM1%20%7C%20Footprinting-orange?style=flat-square )
![Task](https://img.shields.io/badge/Task-06%20%7C%20dnsrecon-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square )
![Purpose](https://img.shields.io/badge/Purpose-Educational%20Only-yellow?style=flat-square )

> **Project:** Week 2 — Project Module 1: Footprinting and Reconnaissance
>
> **Objective:** Enumerate publicly available DNS records for `networkwalks.com`, including name servers, mail servers, SPF records, TXT records, and service records.
>
> **Authorization:** Perform DNS enumeration only against domains and systems you own or have explicit permission to assess.

---

## Table of Contents

- [What Is `dnsrecon`?](#what-is-dnsrecon)
- [What Is DNS Enumeration?](#what-is-dns-enumeration)
- [How It Works](#how-it-works)
- [Command Syntax](#command-syntax)
- [Step-by-Step: Running the Task](#step-by-step-running-the-task)
- [DNS Records That May Be Identified](#dns-records-that-may-be-identified)
- [Reading the Output](#reading-the-output)
- [🔴 Attacker Perspective](#-attacker-perspective)
- [🔵 Defender Perspective](#-defender-perspective)
- [Conclusion](#conclusion)

---

## What Is `dnsrecon`?

`dnsrecon` is a DNS enumeration tool included with Kali Linux. It is used to collect publicly available DNS information associated with a domain.

Depending on the target and DNS configuration, `dnsrecon` may identify:

- IPv4 and IPv6 address records.
- Authoritative name servers.
- Mail-exchange servers.
- Start of Authority information.
- SPF and other TXT records.
- Service records.
- DNS delegation details.
- Public hostnames and subdomains.
- DNS software or version information, when exposed.

For this task, the target domain is:

```text
networkwalks.com
```

The required command is:

```bash
dnsrecon -d networkwalks.com
```

> **Important:** DNS results may change because of record updates, caching, hosting changes, DNS providers, and network configuration. Treat the output as a time-specific observation.

---

## What Is DNS Enumeration?

DNS enumeration is the process of identifying DNS records and hostnames associated with a domain.

DNS records can reveal how a domain handles:

- Website traffic.
- Email delivery.
- Domain administration.
- Service discovery.
- Domain verification.
- Security policies.
- Hosting and infrastructure relationships.

### DNS Enumeration Flow

```text
Kali Linux terminal
        |
        |  dnsrecon -d networkwalks.com
        v
DNS resolver and authoritative DNS infrastructure
        |
        |  Returns publicly available DNS records
        v
DNSRecon identifies and organizes the records
        |
        v
Results are displayed in the terminal
```

DNS enumeration normally queries publicly accessible DNS infrastructure. It should still be performed only within the approved scope of an assessment.

---

## How It Works

When `dnsrecon` is executed, it queries DNS records associated with the target domain and organizes the responses.

The process may include:

1. Identifying address records for the domain.
2. Discovering authoritative name servers.
3. Querying mail-exchange records.
4. Identifying TXT and SPF policies.
5. Querying service records.
6. Reading Start of Authority information.
7. Identifying DNS delegation and configuration details.
8. Displaying the discovered results in the terminal.

The amount of information returned depends on the records that the domain owner has published and the responses allowed by the DNS infrastructure.

---

## Command Syntax

### Basic Syntax

```text
dnsrecon -d domain
```

Required command:

```bash
dnsrecon -d networkwalks.com
```

### Useful Variations

| Command | Purpose |
|---|---|
| `dnsrecon -d networkwalks.com` | Perform standard DNS enumeration. |
| `dnsrecon -d networkwalks.com -a` | Perform reverse lookup operations against discovered addresses where supported. |
| `dnsrecon -d networkwalks.com -t std` | Use standard DNS enumeration. |
| `dnsrecon -d networkwalks.com -t brt -D wordlist.txt` | Attempt permitted subdomain brute-force discovery using a wordlist. |
| `dnsrecon -d networkwalks.com -t srv` | Query service records. |
| `dnsrecon -d networkwalks.com -c dnsrecon.csv` | Save results in CSV format where supported. |
| `dnsrecon -h` | Display the help menu. |

The standard Task 6 command is:

```bash
dnsrecon -d networkwalks.com
```

---

## Step-by-Step: Running the Task

### 1. Open the Terminal

Launch a terminal in Kali Linux.

### 2. Confirm That `dnsrecon` Is Available

```bash
which dnsrecon
```

### 3. Run the Required Command

```bash
dnsrecon -d networkwalks.com
```

### 4. Save the Output

```bash
dnsrecon -d networkwalks.com | tee task-6-dnsrecon.txt
```

This displays the results in the terminal and saves a copy to:

```text
task-6-dnsrecon.txt
```

### 5. Capture Evidence

Take a screenshot showing:

- The Kali Linux terminal.
- The command that was executed.
- The complete DNS enumeration output.
- The terminal prompt or timestamp, if available.

### 6. Organize the Evidence

```text
Task-6/
├── README.md
├── dnsrecon.txt
└── imagepng
|__image.png1
```

---

## DNS Records That May Be Identified

### `A` Record

An `A` record maps a domain name to an IPv4 address.

Example:

```text
networkwalks.com  A  192.232.216.135
```

### `AAAA` Record

An `AAAA` record maps a domain name to an IPv6 address.

### `NS` Record

An `NS` record identifies the authoritative name servers for the domain.

### `MX` Record

An `MX` record identifies the mail servers responsible for receiving email for the domain.

### `SOA` Record

An `SOA`, or Start of Authority, record contains administrative information about the DNS zone, including the primary name server, responsible contact, serial number, and timing values.

### `TXT` Record

A `TXT` record stores text associated with a domain. TXT records may be used for:

- Domain verification.
- SPF email policy.
- DKIM configuration.
- DMARC policy.
- Cloud-service verification.
- Other administrative purposes.

### `SPF` Information

SPF information identifies which mail servers are authorized to send email for the domain. Modern SPF policies are normally published through TXT records.

### `SRV` Record

An `SRV` record identifies the hostname and port used by a particular service.

SRV records can reveal services such as:

- SIP.
- LDAP.
- Kerberos.
- XMPP.
- Microsoft-related services.
- Other application protocols.

### `CNAME` Record

A `CNAME` record creates an alias from one hostname to another hostname. It may reveal relationships between the target domain and an external hosting or service provider.

---

## Reading the Output

A typical result may contain output similar to the following:

```text
[*] Performing General Enumeration of Domain: networkwalks.com

[-] DNSSEC is not configured for networkwalks.com
[*] SOA ns1.example.com 192.0.2.10
[*] NS ns1.example.com 192.0.2.10
[*] NS ns2.example.com 192.0.2.11
[*] MX mail.networkwalks.com 192.0.2.20
[*] TXT networkwalks.com v=spf1 include:example.com ~all
[*] SRV _service._tcp.networkwalks.com
```

The exact output depends on the live DNS configuration.

| Output | Meaning |
|---|---|
| `SOA` | Administrative information for the DNS zone. |
| `NS` | Authoritative name server for the domain. |
| `MX` | Mail server responsible for receiving email. |
| `TXT` | Public text record that may contain verification or policy information. |
| `SRV` | Service name, protocol, priority, weight, port, and target. |
| `A` | IPv4 address record. |
| `AAAA` | IPv6 address record. |
| `CNAME` | Alias pointing to another hostname. |
| `DNSSEC` | Information about DNS Security Extensions. |

---

## Observations from the Laboratory Task

The attached laboratory material indicates that DNS enumeration may reveal information such as:

- Name servers associated with the domain.
- Mail servers and email-routing information.
- SPF and TXT policies.
- Service records.
- DNS software information, including a possible BIND version.
- cPanel-related service records.
- Hosting and email infrastructure relationships.

The exact values should be taken from the output produced during your own execution:

```bash
dnsrecon -d networkwalks.com
```

Do not rely on an old output if the task requires current evidence.

---

## 🔴 Attacker Perspective

From an attacker’s perspective, DNS enumeration provides a broader view of the target’s public infrastructure.

During an **authorized security assessment**, the results may be used to:

- Map the domain’s DNS infrastructure.
- Identify authoritative name servers.
- Identify mail servers and email providers.
- Discover service records and application endpoints.
- Understand hosting and service-provider relationships.
- Review SPF and TXT policies for configuration weaknesses.
- Identify possible subdomains or externally hosted services.
- Correlate DNS records with approved reconnaissance findings.

The task material highlights that mail servers, SPF policies, TXT records, service records, cPanel records, and DNS software information may reveal details about the email and hosting environment.

> **Safety boundary:** DNS records must not be used to launch unauthorized attacks, target mail servers, abuse service endpoints, or test unrelated systems that appear in the output.

---

## 🔵 Defender Perspective

Defenders should enumerate their own DNS records to understand what information is publicly visible and whether the records are correctly configured.

A defensive review should consider:

- Whether all name servers are authorized.
- Whether stale DNS records remain published.
- Whether unused subdomains point to external services.
- Whether mail servers are correctly protected.
- Whether SPF, DKIM, and DMARC policies are configured appropriately.
- Whether TXT records reveal unnecessary internal or vendor information.
- Whether service records expose unnecessary ports or systems.
- Whether DNS software versions are unnecessarily disclosed.
- Whether DNS changes are monitored and reviewed.

| Finding | Recommended Defensive Action |
|---|---|
| Unknown name server | Verify the DNS provider and remove unauthorized delegation. |
| Stale subdomain | Remove the record or secure the associated service. |
| Exposed mail server | Review mail-server security, authentication, and anti-abuse controls. |
| Weak SPF policy | Review authorized senders and avoid overly permissive policies. |
| Missing email protections | Configure SPF, DKIM, and DMARC according to the organization’s mail architecture. |
| Unnecessary TXT record | Remove sensitive or obsolete information. |
| Exposed service record | Confirm that the service is required and restrict access where appropriate. |
| DNS version disclosure | Minimize unnecessary software-version information. |

> **Defensive goal:** Publish only the DNS records required for legitimate business services and continuously monitor changes to the public DNS footprint.

---

## Conclusion

The `dnsrecon` command provides an organized way to enumerate publicly available DNS information for a domain. It can reveal address records, name servers, mail servers, SPF and TXT policies, service records, DNS software details, and relationships with hosting or email providers.

For an authorized security assessment, these findings help create an infrastructure map. For defenders, the same information shows what an external observer can learn and which DNS records may require cleanup, protection, or monitoring.

DNS enumeration is an information-gathering activity, but it must still be performed only within an approved scope. The final report should use the exact output collected during the current execution.

---

<div align="center">

**Week 2 | Project Module 1 | Task 6**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorized%20Only-brightgreen?style=flat-square )

</div>

