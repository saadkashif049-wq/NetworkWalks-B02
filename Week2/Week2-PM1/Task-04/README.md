# Task 4 — Web Application Firewall Detection with `wafw00f`

![Week](https://img.shields.io/badge/Week-02-blue?style=flat-square )
![Module](https://img.shields.io/badge/Module-PM1%20%7C%20Footprinting-orange?style=flat-square )
![Task](https://img.shields.io/badge/Task-05%20%7C%20wafw00f-success?style=flat-square )
![Platform](https://img.shields.io/badge/Platform-Kali%20Linux-557C94?style=flat-square )
![Purpose](https://img.shields.io/badge/Purpose-Educational%20Only-yellow?style=flat-square )

> **Project:** Week 2 — Project Module 1: Footprinting and Reconnaissance
>
> **Objective:** Determine whether `networkwalks.com` is protected by a Web Application Firewall (WAF) and identify the detected WAF technology, if possible.
>
> **Authorization:** Perform reconnaissance only against websites and systems you own or have explicit permission to assess.

---

## Table of Contents

- [What Is `wafw00f`?](#what-is-wafw00f)
- [What Is a WAF?](#what-is-a-waf)
- [How It Works](#how-it-works)
- [Command Syntax](#command-syntax)
- [Step-by-Step: Running the Task](#step-by-step-running-the-task)
- [Reading the Output](#reading-the-output)
- [Observed Result](#observed-result)
- [🔴 Attacker Perspective](#-attacker-perspective)
- [🔵 Defender Perspective](#-defender-perspective)
- [Conclusion](#conclusion)

---

## What Is `wafw00f`?

`wafw00f` is a command-line tool used to identify whether a website is protected by a Web Application Firewall. When possible, it also attempts to identify the WAF product or vendor.

The tool may analyze how a website responds to different requests and compare the responses with known WAF behavior and signatures.

For this task, the target is:

```text
networkwalks.com
```

The required command is:

```bash
wafw00f networkwalks.com
```

> **Important:** WAF detection is based on observable behavior and signatures. A negative result does not prove that no firewall or security control exists.

---

## What Is a WAF?

A **Web Application Firewall**, or WAF, is a security control that monitors and filters HTTP and HTTPS traffic sent to a web application.

A WAF may help detect or block suspicious requests such as:

- SQL injection attempts.
- Cross-site scripting payloads.
- Path traversal attempts.
- Malicious file-upload requests.
- Automated scanning activity.
- Abnormal request patterns.
- Known exploit signatures.

A WAF may operate directly on the web server, in front of the application, through a reverse proxy, or as part of a cloud security service.

### Basic WAF Position

```text
Client request
      |
      v
Web Application Firewall
      |
      |  Inspects and filters HTTP/HTTPS traffic
      v
Web server or web application
      |
      v
Response returned to the client
```

A WAF is not a replacement for secure coding, patch management, authentication controls, network segmentation, or regular security testing.

---

## How It Works

When `wafw00f` is executed, it sends controlled web requests to the target and examines the responses.

### Detection Flow

```text
Kali Linux terminal
        |
        |  wafw00f networkwalks.com
        v
Target website or security gateway
        |
        |  Returns normal or filtered responses
        v
wafw00f compares response behavior
        |
        v
Possible WAF vendor or product is displayed
```

The tool may look for indicators such as:

1. Response status codes.
2. HTTP headers.
3. Cookies and naming patterns.
4. Blocking or challenge pages.
5. Response differences caused by unusual requests.
6. Known signatures associated with WAF products.

Because WAF detection involves sending requests to the target, it should be performed only within an authorized assessment scope.

---

## Command Syntax

### Basic Syntax

```text
wafw00f [options] target
```

Required command:

```bash
wafw00f networkwalks.com
```

### Useful Variations

| Command | Purpose |
|---|---|
| `wafw00f networkwalks.com` | Perform a standard WAF detection scan. |
| `wafw00f -a networkwalks.com` | Attempt to identify all possible WAF products. |
| `wafw00f -v networkwalks.com` | Display verbose detection information. |
| `wafw00f -l` | List supported WAF signatures. |
| `wafw00f -h` | Display the help menu. |

### Recommended Evidence Command

Display the result and save a copy to a text file:

```bash
wafw00f networkwalks.com | tee task-5-wafw00f.txt
```

For a more detailed result, where permitted:

```bash
wafw00f -v networkwalks.com | tee task-5-wafw00f-verbose.txt
```

---

## Step-by-Step: Running the Task

### 1. Open the Terminal

Launch a terminal in Kali Linux.

### 2. Confirm That `wafw00f` Is Available

```bash
which wafw00f
```

### 3. Run the Required Command

```bash
wafw00f networkwalks.com
```

### 4. Save the Output

```bash
wafw00f networkwalks.com | tee task-5-wafw00f.txt
```

### 5. Capture Evidence

Take a screenshot showing:

- The Kali Linux terminal.
- The command that was executed.
- The complete `wafw00f` output.
- The terminal prompt or timestamp, if available.

### 6. Organize the Evidence

```text
Task-5-wafw00f/
├── README.md
├── wafw00f.txt
└── image.png
```

---

## Reading the Output

A typical result may look similar to this:

```text
[*] Checking https://networkwalks.com
[+] The site networkwalks.com is behind ModSecurity (SpiderLabs )
```

The exact output may vary depending on the target's configuration, DNS routing, reverse proxy, CDN, and WAF behavior.

| Output Element | Meaning |
|---|---|
| `Checking` | The target URL being tested. |
| `is behind` | The tool identified behavior associated with a WAF. |
| `ModSecurity` | The detected WAF technology or product family. |
| `SpiderLabs` | The organization associated with the identified ModSecurity technology. |
| `No WAF detected` | No supported WAF signature was identified; this does not prove that no security control exists. |

### Possible Results

#### WAF Detected

The tool identifies a likely WAF product or vendor based on response behavior and known signatures.

#### WAF Not Detected

The tool does not identify a supported WAF signature. The target may still use an unknown, customized, cloud-based, or intentionally hidden security control.

#### Multiple WAFs or Uncertain Result

The target may use multiple layers, a CDN, a reverse proxy, or a security service that produces overlapping indicators.

> **Interpretation rule:** Treat the result as an indicator rather than absolute proof. Detection tools can produce false positives and false negatives.

---

## Observed Result

The attached laboratory material identifies the target as being protected by:

```text
ModSecurity (SpiderLabs)
```

ModSecurity is a widely used open-source Web Application Firewall technology that can be deployed with web servers such as Apache and Nginx or through other hosting and security configurations.

The result suggests that requests sent to the website may pass through a security layer capable of inspecting and filtering web traffic.

> **Important:** The presence of ModSecurity does not guarantee that the website is secure. WAF rules may be incomplete, outdated, incorrectly configured, or unable to detect every type of attack.

---

## 🔴 Attacker Perspective

From an attacker’s perspective, identifying a WAF provides information about the defensive layer protecting the web application.

During an **authorized security assessment**, the finding may be used to:

- Document the presence of a web-security gateway.
- Understand that suspicious requests may be inspected or blocked.
- Interpret blocked responses and challenge pages correctly.
- Plan safe, non-destructive testing within the rules of engagement.
- Distinguish application behavior from WAF-generated responses.
- Record the likely WAF product for the final assessment report.

The task material identifies ModSecurity associated with SpiderLabs as the observed WAF technology.

> **Safety boundary:** WAF detection does not authorize bypass attempts, payload testing, exploitation, evasion, denial-of-service activity, or attacks against the target.

---

## 🔵 Defender Perspective

Defenders should use WAF detection against their own websites to confirm whether the expected security layer is visible and operating correctly.

A defensive review should consider:

- Whether the WAF is deployed in the correct position.
- Whether traffic reaches the WAF before reaching the origin server.
- Whether WAF rules are enabled and regularly updated.
- Whether blocked requests are logged and monitored.
- Whether false positives are reviewed without weakening protection unnecessarily.
- Whether the origin server is protected from direct access.
- Whether the WAF is supported and maintained.
- Whether the application has security controls beyond the WAF.

| Finding | Recommended Defensive Action |
|---|---|
| WAF not detected | Confirm whether the WAF is deployed, reachable, and configured correctly. |
| WAF detected but origin exposed | Restrict direct origin access and allow approved traffic paths only. |
| Outdated WAF rules | Update signatures and review current protection policies. |
| Excessive false positives | Tune rules carefully and monitor changes. |
| No alerting or logging | Forward WAF events to centralized monitoring and incident-response systems. |
| WAF used as the only control | Combine WAF protection with secure coding, patching, authentication, and network controls. |

> **Defensive goal:** Ensure that the WAF is correctly deployed, actively monitored, regularly updated, and supported by secure application and infrastructure practices.

---

## Conclusion

The `wafw00f` command provides a quick way to determine whether a website appears to be protected by a Web Application Firewall. In this task, the observed result identifies **ModSecurity (SpiderLabs)** as the likely WAF protecting `networkwalks.com`.

The result is useful for both reconnaissance and defense. An authorized analyst can document the visible security layer, while defenders can verify that the expected WAF is deployed, monitored, updated, and protecting the origin server.

WAF detection should always be treated as an indication based on observable behavior. It does not prove that the application is fully secure or that every request will be blocked.

---

<div align="center">

**Week 2 | Project Module 1 | Task 4**

![Educational](https://img.shields.io/badge/Use-Educational%20and%20Authorized%20Only-brightgreen?style=flat-square )

</div>

