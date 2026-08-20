# Ethical Hacking and Network Reconnaissance Report

![Organisation](https://img.shields.io/badge/Organisation-Networkwalks-1F4E79?style=flat-square )
![Programme](https://img.shields.io/badge/Programme-Cybersecurity%20Internship-success?style=flat-square )
![Labs](https://img.shields.io/badge/Labs-Footprinting%20%7C%20Zenmap-orange?style=flat-square )
![Scope](https://img.shields.io/badge/Scope-Authorised%20Lab%20Environment-yellow?style=flat-square )
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square )

## Report Information

| Field | Details |
|---|---|
| Organisation | Networkwalks |
| Programme | Cybersecurity Internship |
| Report title | Ethical Hacking and Network Reconnaissance Report |
| Submitted by | Cybersecurity Intern, Saad Kashif |
| Assessment areas | Footprinting and Network Discovery |
| Environment | Authorised educational laboratory |
| Evidence | Command outputs, screenshots, and Zenmap topology data |

## Executive Summary

This report documents two practical cybersecurity laboratories completed during the Networkwalks cybersecurity internship.

The first laboratory focused on **footprinting and reconnaissance**. Multiple tools were used to examine publicly observable information associated with the authorised target domain. The exercise covered domain registration data, web technologies, DNS resolution, HTTP response headers, Web Application Firewall indicators, and DNS records.

The second laboratory focused on **network discovery with Zenmap**, the graphical interface for Nmap. A controlled Ping Scan was performed against an authorised private network to identify live hosts, review local network evidence, and analyse the resulting topology.

The exercises were designed to develop practical skills in reconnaissance, evidence collection, technical interpretation, privacy protection, and professional cybersecurity reporting. No exploitation, credential attacks, persistence, or denial-of-service activity was performed.

## Objectives

The objectives of the laboratories were to:

- Understand the role of reconnaissance in an ethical hacking assessment.
- Identify publicly available domain and web-technology information.
- Review DNS, HTTP, WAF, mail, and service-record data.
- Use Zenmap to identify live hosts on an authorised private network.
- Interpret scan results without overstating their security impact.
- Preserve evidence in a clear and repeatable format.
- Apply responsible handling to IP addresses, MAC addresses, hostnames, and other sensitive information.

## Ethical and Legal Scope

All activities were completed for educational purposes within an authorised laboratory environment. The work was limited to information gathering and controlled network discovery.

The following activities were not performed:

- Exploitation of identified technologies.
- Password attacks or credential testing.
- Unauthorised access attempts.
- Persistence or malware deployment.
- Denial-of-service activity.
- Testing of unrelated public systems.

Sensitive network identifiers have been redacted from public evidence copies. Original evidence should be retained privately for assessment and instructor review.

## Laboratory 1: Footprinting and Reconnaissance

### Methodology

The following tools were used to build a structured view of the authorised target’s public footprint:

| Task | Tool | Primary purpose |
|---:|---|---|
| 1 | `whois` | Review domain registration and registrar information. |
| 2 | `WhatWeb` | Identify publicly visible web technologies and metadata. |
| 3 | `nslookup` | Resolve the domain and identify DNS resolver information. |
| 4 | `curl -I` | Inspect HTTP response headers. |
| 5 | `wafw00f` | Identify possible Web Application Firewall technology. |
| 6 | `dnsrecon` | Enumerate publicly available DNS records and services. |

### Tools and Findings

#### 1. WHOIS Analysis

The WHOIS lookup was used to review publicly available registration details, including registrar information, creation and expiry dates, registration status, name servers, and available administrative contacts.

The results provide administrative context about the domain and may help identify its registrar and DNS provider. WHOIS data can be incomplete or privacy-redacted, so it should be treated as a time-specific observation.

#### 2. Web Technology Fingerprinting

WhatWeb was used to identify technologies exposed by the website. The output was reviewed for web-server software, content-management systems, plugins, libraries, versions, IP information, and other visible metadata.

Technology fingerprinting helps establish an initial application profile. A detected technology or version is not proof of a vulnerability and would require separate authorised validation.

#### 3. DNS Resolution

`nslookup` was used to determine how the authorised domain resolved through DNS. The output was reviewed for the configured resolver, response type, domain name, and returned address information.

DNS results can vary because of caching, hosting changes, proxies, CDNs, and load-balancing configurations. The recorded result therefore represents the environment at the time of testing.

#### 4. HTTP Header Analysis

The following command was used to inspect HTTP response headers without downloading the complete webpage:

```bash
curl -I https://networkwalks.com
```

The response was reviewed for status codes, server banners, cookies, cache controls, redirects, security headers, application indicators, and publicly referenced endpoints.

#### 5. WAF Detection

`wafw00f` was used to determine whether the website appeared to be protected by a Web Application Firewall. The observed result identified a likely ModSecurity deployment associated with SpiderLabs.

This result indicates the presence of a possible protective layer, but it does not confirm that the application is secure or that all malicious requests would be blocked.

#### 6. DNS Record Enumeration

DNSRecon was used to enumerate publicly available DNS information, including:

- Address records.
- Authoritative name servers.
- Mail-exchange records.
- TXT and SPF information.
- Service records.
- SOA information.
- DNS software or hosting indicators where exposed.

DNS enumeration can reveal relationships between web hosting, email services, DNS providers, and other public infrastructure.

## Laboratory 2: Network Discovery with Zenmap

### Methodology

Zenmap was used as the graphical interface for Nmap. The **Ping scan** profile was selected to identify live hosts on an authorised private network without performing a standard port scan.

The evidence confirmed the following scan details:

| Scan element | Observed result |
|---|---|
| Tool | Zenmap / Nmap |
| Nmap version | 7.991 |
| Scan profile | Ping scan |
| Executed target | Recorded exactly from the supplied evidence |
| Address space | 256 IP addresses |
| Live hosts | 4 hosts up |
| MAC evidence | Present in the scan output and redacted in the public report |
| Topology | Captured using the Zenmap Topology view |

The scan command followed the Nmap host-discovery format:

```bash
nmap -sn <authorised-subnet>
```

The target range and all discovered host information were handled as private laboratory evidence.

### Zenmap Results

The scan identified four live hosts within the authorised network range. The output also displayed MAC-address information for local devices where the scan had sufficient Layer 2 visibility and permissions.

The Zenmap topology view provided a graphical representation of the discovered hosts and available network relationships. The topology is an interpretation of scan and route information rather than a guaranteed physical map of the network.

### Interpretation

The presence of a live host indicates only that the host responded to the selected discovery method at the time of testing. It does not establish that the device is vulnerable, misconfigured, or accessible through a particular service.

A professional assessment would require asset-owner confirmation, service-level validation, and explicit approval before any additional scanning or testing.

## Security Observations

| Observation | Security significance |
|---|---|
| Domain registration data was publicly queryable | Administrative and ownership-related information may be available to external observers. |
| Web technologies were fingerprintable | Public technology indicators can help build an application inventory. |
| DNS records exposed infrastructure relationships | DNS data may reveal hosting, email, and service-provider information. |
| HTTP headers returned implementation details | Server, cookie, cache, redirect, and application metadata may be visible. |
| A WAF indicator was detected | A protective web layer appears to be present, but its effectiveness was not tested. |
| Multiple local hosts responded to Zenmap discovery | The authorised private network contained four live hosts at the time of scanning. |
| MAC and hostname information appeared in local evidence | Local network discovery can reveal device-level metadata when visibility permits. |

## Defensive Recommendations

The following recommendations are based on the information exposed during the laboratories:

1. Review publicly visible domain, DNS, web-server, and application metadata.
2. Remove obsolete DNS records and unused subdomains.
3. Minimise unnecessary server and application version disclosure.
4. Review HTTP security headers and cookie attributes.
5. Keep CMS platforms, plugins, frameworks, and web servers patched.
6. Confirm that WAF rules are current, monitored, and supported by secure application controls.
7. Review SPF, DKIM, and DMARC configuration for the organisation’s email environment.
8. Maintain an accurate inventory of authorised network devices.
9. Investigate unknown live hosts through an approved internal process.
10. Protect router, firewall, and network-management interfaces from unauthorised access.
11. Redact private IP addresses, MAC addresses, hostnames, and public IP information before public sharing.

## Evidence Structure

The supporting evidence should be organised in a consistent folder structure:

```text
Networkwalks-B02/
├── README.md
├── W2_FootPrinfting Report.docx
├── Week2 PM1/
│   ├── Task-01-whois/
│   ├── Task-02-whatweb/
│   ├── Task-03-nslookup/
│   ├── Task-04-curl/
│   ├── Task-05-wafw00f/
│   └── Task-06-dnsrecon/
└──Week2-PM5
|___ Zenmap/
    ├── image.png
    ├── image(1).png
    └── README.md
```

## Professional Reporting Principles

The report follows an evidence-based approach. Each conclusion is linked to a command output, screenshot, or topology result. Observations are separated from assumptions, and the presence of a technology or live host is not presented as proof of a vulnerability.

The assessment also applies responsible disclosure principles. Public evidence copies should conceal information that could identify a private network, device, user, or organisation. Original material should be stored securely and shared only with authorised reviewers.

## Conclusion

These laboratories provided practical experience in two important areas of cybersecurity work: external footprinting and internal network visibility.

The footprinting exercise demonstrated how domain registration, DNS, HTTP, web technology, WAF, and service information can be combined to understand a public-facing environment. The Zenmap exercise demonstrated how authorised host discovery can reveal the active devices and topology of a private network.

The most important outcome was learning how to turn raw technical output into structured security evidence. Effective ethical hacking requires more than running tools. It requires clear scope, careful interpretation, privacy protection, accurate documentation, and recommendations that can be understood by both technical and non-technical stakeholders.

## Author

**Saad Kashif**  
Cybersecurity Intern  
Networkwalks Cybersecurity Internship

## References

[1]: https://nmap.org/zenmap/ "Nmap Project — Zenmap GUI"
[2]: https://nmap.org/book/man.html "Nmap Reference Guide"
[3]: https://nmap.org/book/host-discovery.html "Nmap Network Scanning — Host Discovery"
[4]: https://nmap.org/book/output.html "Nmap Network Scanning — Understanding Nmap Output"
[5]: https://nmap.org/download.html "Nmap Official Download Page"

---

<div align="center">

**Networkwalks | Cybersecurity Internship**

![Authorised](https://img.shields.io/badge/Use-Educational%20and%20Authorised%20Only-brightgreen?style=flat-square )

</div>
