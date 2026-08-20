# Zenmap Network Discovery Lab

![Tool](https://img.shields.io/badge/Tool-Zenmap%20%2F%20Nmap-1F4E79?style=flat-square )
![Focus](https://img.shields.io/badge/Focus-Network%20Discovery-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Kali%20Linux-557C94?style=flat-square )
![Use](https://img.shields.io/badge/Use-Authorised%20Labs%20Only-yellow?style=flat-square )

> A practical guide to using Zenmap for authorised host discovery, service enumeration, result interpretation, topology analysis, evidence collection, and professional reporting.

## Table of Contents

- [Overview](#overview)
- [What Is Zenmap?](#what-is-zenmap)
- [Zenmap and Nmap](#zenmap-and-nmap)
- [How Zenmap Works](#how-zenmap-works)
- [Authorisation and Scope](#authorisation-and-scope)
- [Installation](#installation)
- [Finding the Correct Network Range](#finding-the-correct-network-range)
- [Understanding CIDR Notation](#understanding-cidr-notation)
- [First Scan: Ping Scan](#first-scan-ping-scan)
- [Common Zenmap Scan Profiles](#common-zenmap-scan-profiles)
- [Useful Nmap Commands](#useful-nmap-commands)
- [Reading Scan Results](#reading-scan-results)
- [Understanding Host States](#understanding-host-states)
- [Understanding Ports and Services](#understanding-ports-and-services)
- [MAC Address Information](#mac-address-information)
- [Using the Topology View](#using-the-topology-view)
- [Saving Evidence](#saving-evidence)
- [Reporting Format](#reporting-format)
- [Troubleshooting](#troubleshooting)
- [Privacy and Responsible Sharing](#privacy-and-responsible-sharing)
- [Defensive Interpretation](#defensive-interpretation)
- [Conclusion](#conclusion)
- [References](#references)

## Overview

Zenmap is a graphical network-discovery interface used to run Nmap scans and review their results. It is useful for learning network visibility, identifying live hosts, reviewing services, and producing a visual representation of a permitted network environment.

This guide uses a private laboratory network as the example environment. The commands and procedures must be limited to systems that you own or have explicit permission to assess.

## What Is Zenmap?

Zenmap is the official graphical user interface for Nmap. It provides a visual way to configure scans, select scan profiles, review command syntax, compare results, inspect hosts, and display topology information.[1] [2]

Zenmap does not perform a separate type of scanning from Nmap. It uses Nmap as the scanning engine and presents the results through a graphical interface.

Zenmap can help an analyst:

- Identify hosts that are responding on an authorised network.
- Review open, closed, and filtered ports.
- Identify services and possible software versions.
- Perform operating-system detection when authorised and supported.
- Save scan results for later comparison.
- Review discovered systems in a topology view.

## Zenmap and Nmap

| Feature | Nmap | Zenmap |
|---|---|---|
| Interface | Command line | Graphical user interface |
| Engine | Performs the scan directly | Uses Nmap in the background |
| Automation | Excellent for scripts and scheduled work | More limited than command-line workflows |
| Learning curve | Requires command knowledge | Easier for beginners and visual learners |
| Result review | Terminal output and saved files | Tabs, profiles, host panels, and topology view |
| Best use | Advanced scanning, scripting, and repeatable operations | Interactive lab work and visual analysis |

The same Zenmap scan profile can usually be represented by an Nmap command. For example, the Zenmap **Ping scan** profile corresponds to a command similar to:

```bash
nmap -sn 192.168.1.0/24
```

## How Zenmap Works

A typical Zenmap workflow follows this sequence:

```text
Identify the authorised target range
              |
              v
Select a scan profile
              |
              v
Zenmap builds the Nmap command
              |
              v
Nmap sends permitted discovery or service probes
              |
              v
Zenmap displays hosts, ports, services, and topology
              |
              v
The analyst records evidence and interprets the results
```

For a local network, Nmap may use local network discovery methods such as ARP. For remote targets, it may use other host-discovery probes depending on the scan options, network path, permissions, and firewall behaviour.

A scan result is an observation of network behaviour at a particular time. It is not proof that a host is secure or insecure.

## Authorisation and Scope

Before starting a scan, confirm the following:

| Scope item | Required confirmation |
|---|---|
| Target ownership | The network or hosts belong to you or are explicitly authorised for testing. |
| Scan type | The approved activity includes host discovery, port scanning, or service detection as applicable. |
| Time window | The scan is performed during the permitted period. |
| Rate and intensity | The scan is appropriate for the network and will not create unnecessary load. |
| Evidence handling | Screenshots, IP addresses, MAC addresses, and hostnames are stored securely. |

Do not scan public ranges, workplace networks, school networks, neighbours’ devices, or cloud infrastructure without written permission.

## Installation

### Windows

1. Download Nmap from the official Nmap download page.
2. Run the Windows installer.
3. Keep Zenmap selected during installation if the installer provides that option.
4. Start Zenmap from the Windows Start menu.
5. Run Zenmap as Administrator when the lab requires MAC-address visibility or additional discovery permissions.

Official download page:

```text
https://nmap.org/download.html
```

### Kali Linux

Nmap is commonly available on Kali Linux. Confirm its availability with:

```bash
which nmap
nmap --version
```

If Zenmap is installed, confirm it with:

```bash
which zenmap
```

The exact package availability may vary between operating-system releases. Use the official operating-system package-management process and do not install software from untrusted sources.

## Finding the Correct Network Range

Do not guess the target subnet. Identify the active network interface first.

### Windows

Open Command Prompt and run:

```cmd
ipconfig
```

Example:

```text
IPv4 Address. . . . . . . . . . . : 192.168.1.27
Subnet Mask . . . . . . . . . . . : 255.255.255.0
Default Gateway . . . . . . . . . : 192.168.1.1
```

With this configuration, the usual /24 network target is:

```text
192.168.1.0/24
```

### Linux

Run either command:

```bash
ip addr
```

or:

```bash
ip route
```

Example:

```text
192.168.1.0/24 dev eth0 src 192.168.1.40
```

The local network is `192.168.1.0/24` in this example.

### Virtual Machines and NAT

A virtual machine using NAT may receive an address from a separate virtual subnet. That subnet is not necessarily the same as the physical Windows or Wi-Fi network.

If the scan is being run from a Kali virtual machine and the target is the physical LAN, use a **Bridged Adapter** only when permitted and when the VM must participate on the same authorised network. Running Zenmap directly on the physical Windows computer is usually simpler for a Windows-based lab.

## Understanding CIDR Notation

CIDR notation describes a network and its prefix length.

| CIDR | Approximate address space | Common use |
|---|---:|---|
| `/32` | 1 address | Single host |
| `/24` | 256 addresses | Typical small LAN |
| `/16` | 65,536 addresses | Larger private network |

For a `/24` network, use the network address rather than a host address where possible.

Correct network notation:

```text
192.168.18.0/24
```

A command may still accept a host address with a `/24` suffix, such as:

```text
192.168.18.1/24
```

However, the clearer professional form is the normalised network address:

```text
192.168.18.0/24
```

When documenting a lab, record the command exactly as executed and separately state the normalised network notation if they differ.

## First Scan: Ping Scan

A Ping scan is a host-discovery scan. It attempts to identify which hosts are responding without performing a standard port scan.

### Zenmap Procedure

1. Open Zenmap.
2. Enter the authorised subnet in the **Target** field.
3. Select **Ping scan** from the **Profile** menu.
4. Review the generated command.
5. Confirm that the target is within scope.
6. Click **Scan**.
7. Wait for the scan to finish.
8. Review the **Nmap Output**, **Hosts**, and **Topology** tabs.

Example target:

```text
192.168.1.0/24
```

Example generated command:

```bash
nmap -sn 192.168.1.0/24
```

### Command-Line Equivalent

```bash
nmap -sn 192.168.1.0/24
```

The final line may look similar to:

```text
Nmap done: 256 IP addresses (4 hosts up ) scanned in 3.39 seconds
```

Record the number returned by your own scan. Do not copy the example value.

## Common Zenmap Scan Profiles

| Profile | Typical command | Purpose |
|---|---|---|
| Ping scan | `nmap -sn target` | Identify live hosts without a normal port scan. |
| Quick scan | `nmap -T4 -F target` | Quickly check common ports. |
| Regular scan | `nmap target` | Perform a standard TCP port scan. |
| Intense scan | `nmap -T4 -A -v target` | Aggressive service, script, and operating-system detection. Use only when authorised. |
| Intense scan plus UDP | `nmap -sS -sU -T4 -A -v target` | Combines TCP and UDP checks with advanced detection. Use only in a controlled lab. |
| Quick scan plus version detection | `nmap -sV -T4 target` | Identify services and possible versions on discovered ports. |

Scan profiles can generate significant traffic. Begin with the least intrusive scan that meets the lab objective.

## Useful Nmap Commands

### Host Discovery

```bash
nmap -sn 192.168.1.0/24
```

### Scan One Host

```bash
nmap 192.168.1.27
```

### Service and Version Detection

```bash
nmap -sV 192.168.1.27
```

### Operating-System Detection

```bash
nmap -O 192.168.1.27
```

Operating-system detection often requires elevated privileges and may not be accurate. Use it only when it is included in the approved scope.

### Save Normal Output

```bash
nmap -sn 192.168.1.0/24 -oN zenmap-ping-scan.txt
```

### Save XML Output

```bash
nmap -sn 192.168.1.0/24 -oX zenmap-ping-scan.xml
```

### Save All Standard Formats

```bash
nmap -sn 192.168.1.0/24 -oA zenmap-ping-scan
```

This creates normal, XML, and grepable output files.

## Reading Scan Results

A typical host-discovery result may look similar to this:

```text
Starting Nmap 7.991
Nmap scan report for 192.168.1.1
Host is up (0.0060s latency).
MAC Address: AA:BB:CC:DD:EE:FF (Example Vendor)

Nmap scan report for 192.168.1.27
Host is up (0.065s latency).
MAC Address: 11:22:33:44:55:66 (Example Vendor)

Nmap done: 256 IP addresses (2 hosts up) scanned in 3.39 seconds
```

| Output | Meaning |
|---|---|
| `Starting Nmap` | The Nmap version used for the scan. |
| `Nmap scan report for` | A host was identified during the scan. |
| `Host is up` | The host responded to the discovery method used. |
| Latency value | Approximate response time observed by Nmap. |
| `MAC Address` | Layer-2 hardware address and possible vendor information when available. |
| `Nmap done` | Summary of the scanned address space and live-host count. |

The output may not show a hostname for every device. Lack of a hostname does not mean that the device is inactive.

## Understanding Host States

Nmap commonly reports states such as:

| State | Meaning |
|---|---|
| `up` | The host responded to at least one discovery probe. |
| `down` | No response was received using the selected discovery methods. |
| `open` | An application is listening on a port. |
| `closed` | The port is reachable but no application is listening. |
| `filtered` | A firewall or filtering device prevents Nmap from determining the port state. |

Host-discovery results depend on firewalls, wireless isolation, VPNs, routing, device power state, and the selected scan method. A host marked `down` may still exist.

## Understanding Ports and Services

A port is a logical communication endpoint. Services use ports to receive network traffic.

Common examples include:

| Port | Common service |
|---:|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 445 | SMB |
| 3389 | Remote Desktop Protocol |

Port numbers alone do not prove which service is running. Version detection and service banners may provide additional evidence, but those scans should be explicitly authorised.

## MAC Address Information

A MAC address identifies a network interface at the local network layer. Nmap may display MAC addresses during local-network discovery when it has the necessary permissions and the target is on the same Layer 2 network.

Example:

```text
MAC Address: AA:BB:CC:DD:EE:FF (Example Vendor)
```

On Windows, the local ARP table can be reviewed with:

```cmd
arp -a
```

The local computer’s own physical address can be found with:

```cmd
ipconfig /all
```

MAC addresses are generally not visible across routers. A remote scan may show the gateway’s MAC address rather than the destination device’s actual MAC address.

## Using the Topology View

Zenmap’s **Topology** tab presents discovered hosts graphically. Depending on the scan type and available route information, it may display the local scanning computer, routers, hosts, and traceroute relationships.

### Topology Procedure

1. Complete an authorised scan.
2. Open the **Topology** tab.
3. Allow the topology view to load.
4. Enable the **Legend** to understand the symbols.
5. Review the displayed hosts and connections.
6. Use the zoom and layout controls to improve readability.
7. Select **Save Graphic** when the view is ready.
8. Export the topology in the format required by the lab, such as PDF or PNG.

### Topology Interpretation

The topology legend may distinguish:

- Hosts that were not port scanned.
- Hosts with fewer than three open ports.
- Hosts with three to six open ports.
- Hosts with more than six open ports.
- Routers, switches, wireless access points, firewalls, and filtered hosts.
- Primary and alternate traceroute paths.

The topology diagram should not be treated as a perfect physical network map. It is a visual interpretation of the scan results and available route information.

## Saving Evidence

A professional lab submission should preserve both the raw output and visual evidence.


Each screenshot should show the command, target scope, scan profile, result, and date or terminal context where practical.

## Reporting Format

A concise Zenmap report should include the following sections:

### 1. Objective

Explain that the lab identified live hosts and reviewed the topology of an authorised network.

### 2. Scope

State the exact network range, ownership or authorisation, scan date, platform, and scan profile.

### 3. Method

Document the Zenmap profile and the equivalent Nmap command.

### 4. Results

Record the number of addresses scanned, live hosts discovered, IP addresses, MAC evidence, and topology observations.

### 5. Interpretation

Explain what the results mean and clearly distinguish observations from confirmed vulnerabilities.

### 6. Recommendations

Recommend asset-inventory reconciliation, removal of unknown devices, segmentation where appropriate, firewall review, and regular authorised discovery checks.

### 7. Evidence

Attach the raw output, screenshots, and topology export. Redact sensitive details before public sharing.

Example results table:

| Item | Recorded result |
|---|---|
| Tool | Zenmap / Nmap |
| Profile | Ping scan |
| Target | Authorised private subnet |
| Addresses scanned | Record the value from the scan output |
| Live hosts | Record the value from the scan output |
| MAC evidence | Present or not present; redact values for public sharing |
| Topology | Captured and exported |

## Troubleshooting

### No Hosts Are Detected

Confirm that the target matches the active Wi-Fi or Ethernet adapter. Check that devices are powered on and connected to the same network.

### NAT and Bridged Networking Are Confused

A NAT-enabled VM may be on a virtual subnet that is different from the physical LAN. Run Zenmap on the physical Windows machine or use bridged networking in an authorised lab when the VM must be on the same network.

### MAC Addresses Are Missing

Run Zenmap with appropriate administrative permissions and confirm that the target is on the same local network. Use `arp -a` as a supporting check on Windows.

### Only the Router Appears

The network may use guest-network isolation, wireless client isolation, VPN routing, or host firewalls that block discovery traffic. Use a private lab network and review the device and firewall configuration without disabling security controls unnecessarily.

### The Topology View Is Empty

Confirm that the scan completed successfully, that the target range is correct, and that the scan produced host or route information that Zenmap can display.

### Zenmap Cannot Start

Verify that Nmap and Zenmap are installed correctly, reopen the application with the necessary permissions, and confirm that the target uses valid CIDR notation such as:

```text
192.168.1.0/24
```

## Privacy and Responsible Sharing

Before uploading a Zenmap screenshot to LinkedIn, GitHub, or another public platform, create a redacted copy.

Hide or remove:

- Private IP addresses.
- Public or WAN IP addresses.
- MAC addresses.
- Computer names and hostnames.
- Wi-Fi network names.
- Usernames and file paths.
- Router and access-point details.
- Any information belonging to another person or organisation.

The original evidence should be retained privately for the instructor or assessor. Public posts should focus on the learning outcome, scan type, number of discovered hosts, and professional methodology rather than exposing the real network layout.

## Defensive Interpretation

A Zenmap scan is also useful for defenders. An organisation can use authorised discovery to compare live devices against its approved asset inventory.

A defensive review should ask:

- Are all responding devices known and assigned to an owner?
- Are unnecessary services disabled?
- Are network segments separated appropriately?
- Are router and firewall management interfaces protected?
- Are guest and IoT devices isolated where appropriate?
- Are unexpected devices investigated through an approved process?
- Are discovery checks repeated periodically?

The presence of a live host does not mean that it is vulnerable. It indicates only that the host responded to the selected discovery method at the time of the scan.

## Conclusion

Zenmap provides an accessible way to learn Nmap-based network discovery and interpret scan results through a graphical interface. A disciplined workflow begins with authorisation and scope, identifies the correct network range, selects the least intrusive scan that meets the objective, records the exact command and output, and protects the collected evidence.

The most important professional habit is accuracy. Always record the target exactly as scanned, distinguish an executed host-address command from the normalised network notation, verify numeric claims against the raw output, and redact sensitive network information before public sharing.

## References

[1]: https://nmap.org/zenmap/ "Nmap Project — Zenmap GUI"
[2]: https://nmap.org/book/man.html "Nmap Reference Guide"
[3]: https://nmap.org/book/host-discovery.html "Nmap Network Scanning — Host Discovery"
[4]: https://nmap.org/book/output.html "Nmap Network Scanning — Understanding Nmap Output"
[5]: https://nmap.org/download.html "Nmap Official Download Page"

---

<div align="center">

**Zenmap Network Discovery Lab**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorised%20Only-brightgreen?style=flat-square )

</div>
