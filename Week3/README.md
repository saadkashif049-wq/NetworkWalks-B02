<div align="center">

# 🔐 Week 3 – Internship at Networkwalks
## Password Cracking

![Platform](https://img.shields.io/badge/Platform-Browser%20%2F%20Web-FF6F00?style=for-the-badge&logo=googlechrome&logoColor=white)
![Provider](https://img.shields.io/badge/Provider-Networkwalks%20Academy-B71C1C?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Cybersecurity%20%7C%20Ethical%20Hacking-2E7D32?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

</div>

---

## 📖 Introduction

During **Week 3** of my **Cybersecurity internship at Networkwalks**, I studied **Password Cracking** — the process of recovering a password from protected data instead of knowing it directly. This is a core topic in Ethical Hacking, used by security professionals to test how resistant real systems and files are against attackers, and to demonstrate why weak passwords are a serious risk.

---

## 🧠 What is Password Cracking?

**Password cracking** is the process of recovering passwords from data that has been stored or transmitted in a protected form — usually a **hash**. Instead of guessing the password directly against a live system (which is slow and often blocked), attackers or security testers extract the underlying hash and try to match it offline.

It is widely used in:
- 🛡️ **Security auditing** — testing whether an organization's passwords are strong enough
- 🕵️ **Penetration testing** — simulating real attacker behavior in authorized environments
- 🎓 **Training & awareness** — showing why weak passwords fail almost instantly

---

## 🔒 Hashing vs Encryption

| Concept | Direction | Purpose |
|---|---|---|
| 🔑 **Encryption** | Two-way (reversible with the right key) | Protects data so it can be decrypted later |
| 🧬 **Hashing** | One-way (cannot be reversed) | Verifies data/passwords without storing them in plain text |

A password-protected file (like a PDF) stores a **hash** of its password. To "crack" it, a tool hashes many guesses using the same method and checks for a match — it never actually "decrypts" the password out of the hash.

---

## ⚔️ Common Password Cracking Methods

| Method | How It Works | Speed |
|---|---|---|
| 📖 **Dictionary Attack** | Tries every word from a pre-made wordlist (e.g., common passwords, leaked password databases) | Fast for weak/common passwords |
| 🔢 **Brute-Force Attack** | Tries every possible character combination systematically | Very slow, but guaranteed eventually |
| 🧩 **Rule-Based / Mask Attack** | Takes wordlist entries and mutates them (adds numbers, capitals, symbols) | Balances speed and coverage |
| 🎯 **Rainbow Table Attack** | Uses precomputed hash tables to instantly look up matches | Extremely fast, but needs huge storage |

---

## 🧰 Tools Commonly Used for Password Cracking

- 🕵️ **John the Ripper (JTR)** — industry-standard CLI tool supporting many hash and file formats (PDF, ZIP, Office docs, etc.)
- 🖥️ **Johnny** — GUI front-end for John the Ripper
- 🌐 **Networkwalks Hash Calculator** — browser-based tool to extract a crackable hash from files like PDFs, entirely client-side
- 🌐 **Networkwalks Password Cracker (Dictionary Attack Lab)** — browser-based dictionary attack tool that mirrors John the Ripper's logic visually

---

## 🎯 Why This Matters

This topic reinforces one of the most important lessons in cybersecurity: **a password is only as strong as its resistance to being guessed.** Common or short passwords (like `password1`) can be cracked in seconds using a small wordlist, while long, random, unique passwords make dictionary and brute-force attacks impractical — even for attackers with significant computing power.

---

## ⚠️ Disclaimer

Password cracking techniques and tools discussed here were studied and practiced strictly for **educational and internship training purposes**, in a controlled environment using files created specifically for these exercises. These techniques should only ever be applied to **systems and files you own or are explicitly authorized to test.**

---

<div align="center">

### 🎓 Week 3 · Networkwalks Internship · Password Cracking

![Made with](https://img.shields.io/badge/Made%20with-🌐%20Browser--Based%20Tools-informational?style=flat-square)
![Skill](https://img.shields.io/badge/Skill-Password%20Auditing-informational?style=flat-square)

</div>
