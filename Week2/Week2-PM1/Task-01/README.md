# Task 1 — Domain Registration Reconnaissance with `whois`

![Week](https://img.shields.io/badge/Week-02-blue?style=flat-square )
![Module](https://img.shields.io/badge/Module-PM1%20%7C%20Footprinting-orange?style=flat-square )
![Task](https://img.shields.io/badge/Task-01%20%7C%20whois-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square )
![Purpose](https://img.shields.io/badge/Purpose-Educational%20Only-yellow?style=flat-square )

> **Project:** Week 2 — Project Module 1: Footprinting and Reconnaissance
>
> **Objective:** Query the public domain-registration record for `networkwalks.com` and identify available registration, registrar, expiry, and name-server information.
>
> **Authorization:** Perform reconnaissance only against domains and systems you own or have explicit permission to assess.

---

## Table of Contents

- [What Is `whois`?](#what-is-whois)
- [How It Works](#how-it-works)
- [Command Syntax](#command-syntax)
- [Step-by-Step: Running the Task](#step-by-step-running-the-task)
- [Reading the Output](#reading-the-output)
- [Information That Can Be Identified](#information-that-can-be-identified)
- [🔴 Attacker Perspective](#-attacker-perspective)
- [🔵 Defender Perspective](#-defender-perspective)
- [Conclusion](#conclusion)

---

## What Is `whois`?

`whois` is a command-line utility used to query publicly available registration information for internet domains and IP address ranges.

A WHOIS query may reveal information such as:

- The domain registrar.
- Domain creation and registration dates.
- Domain expiry date.
- Updated date.
- Registration status.
- Authoritative name servers.
- Registrar abuse contacts.
- Registrant details, when privacy protection is not enabled.

For this task, the target domain is:

```text
networkwalks.com
```

The purpose of the lookup is to understand what registration and ownership-related information is publicly available before conducting any further authorized assessment.

> **Important:** WHOIS data may be partially hidden or redacted because of privacy services, regional regulations, registrar policies, or changes in WHOIS/RDAP availability.

---

## How It Works

When the following command is executed:

```bash
whois networkwalks.com
```

The `whois` client sends a query to an appropriate WHOIS server. The response contains registration information associated with the domain, if that information is publicly available.

### Query Flow

```text
Kali Linux terminal
        |
        |  whois networkwalks.com
        v
WHOIS or registration service
        |
        |  Searches the domain registration database
        v
Registration record returned
        |
        v
whois displays the available information
```

The command does not exploit the website or access private systems. It retrieves registration information that has been made available through a public registration service.

---

## Command Syntax

### Basic Syntax

```text
whois [options] domain-or-IP
```

Required command:

```bash
whois networkwalks.com
```

### Useful Variations

| Command | Purpose |
|---|---|
| `whois networkwalks.com` | Query the registration record for the domain. |
| `whois 192.232.216.135` | Query registration information for an IP address. |
| `whois -H networkwalks.com` | Hide lengthy legal disclaimers where supported. |
| `whois --help` | Display available command options. |

The exact output and available options may vary depending on the installed WHOIS client and the registration service responding to the query.

---

## Step-by-Step: Running the Task

### 1. Open the Terminal

Launch a terminal in Kali Linux.

### 2. Confirm That `whois` Is Available

```bash
which whois
```

If the command is not available, verify that the WHOIS utility is installed and that the system's package repositories are configured correctly.

### 3. Run the Required Command

```bash
whois networkwalks.com
```

### 4. Save the Output to a Text File

Use `tee` to display the result and save a copy at the same time:

```bash
whois networkwalks.com | tee task-1-whois.txt
```

This creates the following evidence file:

```text
task-1-whois.txt
```

### 5. Capture a Screenshot

Take a screenshot showing:

- The Kali Linux terminal.
- The command that was executed.
- The complete WHOIS output.
- The terminal prompt or timestamp, if available.

### 6. Organize the Evidence

```text
Task-1-whois/
├── README.md
├── task-1-whois.txt
└── screenshots/
    └── whois-networkwalks.png
```

---

## Reading the Output

The output may contain different fields depending on the registrar and the type of registration service used. Common fields include the following:

| Field | Meaning |
|---|---|
| `Domain Name` | The domain being queried. |
| `Registrar` | The company responsible for registering or managing the domain. |
| `Creation Date` | The date on which the domain was originally registered. |
| `Updated Date` | The most recent date on which the registration record was changed. |
| `Registry Expiry Date` | The date on which the registration is scheduled to expire. |
| `Name Server` | An authoritative DNS server responsible for the domain. |
| `Domain Status` | The current registration status and transfer-related restrictions. |
| `Registrant Organization` | The registered organization, if publicly disclosed. |
| `Registrant Country` | The country associated with the registrant, if available. |
| `Registrar Abuse Contact Email` | An abuse-reporting contact provided by the registrar. |
| `DNSSEC` | Information about whether DNS Security Extensions are enabled. |

### Privacy-Redacted Information

Some records may contain text such as:

```text
REDACTED FOR PRIVACY
```

This means that the registrar or privacy-protection provider has intentionally hidden personal registration details. Redaction does not necessarily mean that the domain has no owner or that the registration is invalid.

### Name Servers

Name-server entries identify the DNS infrastructure responsible for answering queries for the domain. For example, name servers may reveal the DNS provider or hosting company used by the domain.

A separate DNS query can be performed with:

```bash
nslookup -type=NS networkwalks.com
```

---

## Information That Can Be Identified

A WHOIS lookup can help an authorized analyst build an initial administrative profile of a domain.

| Category | Possible Observation | Security Relevance |
|---|---|---|
| Registrar | The company managing the domain registration | Identifies the registration provider and abuse channel |
| Creation date | The approximate age of the domain | Helps establish domain history and credibility |
| Expiry date | When the registration is due for renewal | Useful for administrative monitoring and defensive ownership checks |
| Name servers | The DNS providers responsible for the domain | May reveal hosting or DNS infrastructure |
| Status codes | Transfer and registration restrictions | Indicates the current administrative state |
| Contact details | Public administrative or abuse contacts | Provides a route for reporting suspicious activity |
| Privacy protection | Whether registrant details are redacted | Shows how much ownership information is publicly visible |

---

## 🔴 Attacker Perspective

From an attacker’s perspective, WHOIS is an early reconnaissance tool because it can reveal administrative and infrastructure information without directly attacking the target website.

During an **authorized security assessment**, the information may be used to:

- Identify the registrar managing the domain.
- Identify authoritative name servers and possible DNS providers.
- Understand the approximate age and registration history of the domain.
- Find publicly visible administrative or abuse contacts.
- Identify registration patterns shared with other approved assets.
- Understand which organization or provider may be responsible for the infrastructure.

The task material notes that the domain’s name servers can reveal its hosting or DNS provider. Registration dates and public contacts may also provide useful context during an authorized assessment.

> **Safety boundary:** Public registration information must not be used for harassment, social engineering, credential attacks, or unauthorized access. Any follow-up testing must remain within the approved scope and rules of engagement.

---

## 🔵 Defender Perspective

Defenders should perform WHOIS and registration-record reviews against their own domains to determine what administrative information is publicly visible.

A defensive review should consider:

- Whether the domain is registered with the intended registrar.
- Whether the registration expiry date is monitored.
- Whether privacy protection is required for personal or sensitive details.
- Whether public contact information is accurate and appropriate.
- Whether the listed name servers belong to approved DNS providers.
- Whether old or unauthorized name servers remain associated with the domain.
- Whether domain-locking and transfer protections are enabled.

| Finding | Recommended Defensive Action |
|---|---|
| Unexpected registrar | Confirm ownership and investigate unauthorized registrar changes. |
| Approaching expiry date | Enable renewal monitoring and confirm payment or renewal contacts. |
| Public personal information | Enable registrar privacy protection where appropriate. |
| Unknown name server | Verify the DNS provider and remove unauthorized delegation. |
| Missing transfer protection | Enable domain locking and multi-factor authentication. |
| Unmonitored registrar account | Configure account alerts and review administrative access regularly. |

> **Defensive goal:** Keep domain ownership, renewal, DNS delegation, and registrar access under continuous administrative control.

---

## Conclusion

The `whois` command provides an efficient first look at the public registration information associated with a domain. For `networkwalks.com`, the lookup can reveal registration details, registrar information, important dates, name servers, status values, and any publicly available contacts.

WHOIS information is valuable for both reconnaissance and defense. An authorized analyst can use it to understand the administrative ownership and DNS relationships of a domain, while defenders can use the same information to identify unnecessary exposure, prevent unauthorized changes, and maintain control of domain registration assets.

Always treat the returned information as publicly available administrative data and ensure that all follow-up activity remains authorized.

---

<div align="center">

**Week 2 | Project Module 1 | Task 1**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorized%20Only-brightgreen?style=flat-square )

</div>



