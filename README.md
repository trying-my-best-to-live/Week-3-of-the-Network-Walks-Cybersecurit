# 🔐 Cracking & Password Recovery

**Week 3 of the Network Walks Cybersecurity Internship — password cracking, PDF hash recovery and flag capture**

![Skill-Cybersecurity](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000)
![John](https://img.shields.io/badge/John%20the%20Ripper-1.9.0-jumbo-9C27B0?style=flat-square&labelColor=C00000)
![Johnny](https://img.shields.io/badge/Johnny-GUI-2.2-1565C0?style=flat-square&labelColor=C00000)
![NetworkWalks](https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000)
![PDF](https://img.shields.io/badge/PDF%201.7%20%2B%20AES-128-8B4513?style=flat-square&labelColor=C00000)
![Flags](https://img.shields.io/badge/Flags-Captured-2E7D32?style=flat-square&labelColor=C00000)

---

## 📌 Project Overview

This repository covers **Week 3 of the Network Walks Cybersecurity Internship**:

the phase in which learning about a system stops at *reading* it and starts at
*breaking* it — by turning a password-protected PDF into a hash and then
recovering the password with the tools at hand.

Three lab PDFs were encrypted with password protection, and the recovered
passwords were used to open the documents and capture the flags they contained.

Two modules were required:

| Module | Phase | Scope |
| --- | --- | --- |
| **W3-PM1** | Password Cracking with JTR | `My Locked PDF1.pdf` — John the Ripper + Johnny |
| **W3-PM2** | Password Cracking with Networkwalks Tools | `My Locked PDF1 (NW lab)` — Hash Calculator + Password Cracker |

Three flags were captured during the week and are recorded in
`W3-PM-FINAL - Cracking & Password Recovery - Mazen Ashraf.pdf`.

---

## 🎯 Objectives

- Understand how password-protected PDFs store their encryption key as a hash
- Extract a crackable `$pdf$...` hash from a protected PDF
- Run a dictionary attack with John the Ripper / Johnny
- Run a dictionary attack with the Networkwalks web tools
- Open the decrypted PDF and read the flag
- Cross-validate the recovered password with two independent tools

---

## 🛡️ Scope & Authorization

| Target | Authorization |
| --- | --- |
| `networkwalks.com` lab PDFs (via Networkwalks tools) | **Written permission** from Network Walks as part of the internship program |
| `My Locked PDF*.pdf` lab files | **My own** lab material / provided by the course |

---

## 🧰 Tools Used

| Tool | Purpose |
| --- | --- |
| `john` | John the Ripper 1.9.0-jumbo-1 — PDF cracking engine |
| `johnny` | Johnny GUI — graphical front end for John |
| OnlineHashCrack | PDF hash extractor — `$pdf$` hash output |
| Networkwalks Hash Calculator | Computes hashes / extracts crackable PDF hash |
| Networkwalks Password Cracker | Web dictionary attack against a PDF hash |
| `rockyou.txt` | Wordlist used by John the Ripper |

---

## 🪜 Activities Performed

### 1. Password Cracking with JTR — W3-PM1

**Task:** Crack the password of the protected PDF using JTR John and JTR Johnny.

- Downloaded John the Ripper 1.9.0-jumbo-1 (Windows) and Johnny GUI 2.2 (Windows).
- Uploaded the protected PDF to the OnlineHashCrack PDF Hash Extractor and saved the hash in `hash1.txt`.
- Opened the hash in Johnny, started the attack, and confirmed the recovered password.
- Re-ran the same hash with John the Ripper directly using the `rockyou` wordlist.

**Result:** the protected PDF password was recovered and the document could be opened.

### 2. Password Cracking with Networkwalks Tools — W3-PM2

**Task:** Crack the password of the protected PDF using the Networkwalks Hash Calculator and Password Cracker.

- Opened the Networkwalks Hash Calculator, uploaded the protected PDF, and copied the `$pdf$` hash.
- Pasted the hash into the Networkwalks Password Cracker and started the dictionary attack.
- The tool reported `Tried: 91 / 100, 9 pw/s, 91% [*]` and then:
  `[+] MATCH password1 ... PASSWORD CRACKED SUCCESSFULLY password1 This is the PDF password.`
- Opened the PDF with the recovered password — the document read successfully and the flag was captured.

### 4.4 Flags Captured — Week 3

| Flag | Content | Source PDF | Recovered Password |
| --- | --- | --- | --- |
| **Flag 1** | `nw{networkwalks_flag1_jtr_270521_1}` | `My Locked PDF1 (for NW Tools Lab WK3-PM2).pdf` | `password1` |
| **Flag 2** | `nw{networkwalks_persistence_jtr_270521}` | `My Locked PDF2.pdf` | `password1` |
| **Flag 3** | `nw{networkwalks_flag_260821_1}` | `My Locked PDF3.pdf` | `1qaz2wsx` |

### 4.5 Optional Module — W3-OPTIONAL1 (AI-assisted JTR crack via HexStrike MCP)

**Task:** Crack the password of the target PDF using JTR John with the help of the AI (Hexstrike-AI MCP with Claude desktop in Kali Linux).

The HexStrike MCP server was started locally, and the JTR tool (`john_crack`) was invoked through the HexStrike backend with the PDF hash and the rockyou wordlist:

```
john --format=pdf --wordlist=C:\john\rockyou.txt C:\john\opt1_hash.txt
```

Result: `Loaded 1 password hash (PDF [MD5 SHA2 RC4/AES 32/64])` → `password1 (?)` → `return_code: 0, success: true`.

The recovered password was then used to open the target PDF and capture the flag.

## 📊 Findings & Risk Analysis

| # | Finding | Evidence | Impact | Level |
| --- | --- | --- | --- | --- |
| 1 | PDF password recoverable with dictionary attack | Networkwalks Password Cracker: `91/100 words tried`, match found | Weak passwords are breakable in seconds | ● Critical |
| 2 | Same hash confirmed by independent tool | John the Ripper: `1 password hash cracked, 0 left` | Cross-validation of the recovery | ● Low |
| 3 | Recovered password allowed flag read | Flags printed inside decrypted PDFs | Real flag material captured | ● High |

---

## ✅ Recommendations

1. Use long, random passwords for encrypted documents.
2. Prefer passphrase based passwords over dictionary words.
3. Use key management / hardware tokens for high value material.
4. Restrict local network access to authorised devices only.

---

## 🐞 Problems Encountered & Solutions

- **John hash field order.** John's PDF hash format is `$pdf$V*R*keylen*P*EM*id_len*id*u_len*u*o_len*o`. Feeding the fields in the wrong order let the hash load but never crack — the fix was to emit the fields in John's exact order.
- **Hash extraction.** The hash must start with `$pdf$` and must not contain stray characters such as a leading `b'`.
- **Dictionary coverage.** The Networkwalks built-in list contained `password1`, `1qaz2wsx` and others, which allowed fast verification of the recovered passwords.

---

## 💡 What I Learned

1. Password protected files are only as strong as the password.
2. Hash based cracking tools turn weak passwords into fast, measurable attacks.
3. Independent tools agreeing on a recovered password is strong validation.
4. The `$pdf$` hash format is specific to PDF encryption and must be handled carefully.

---

## 📁 Repository Contents

```
Week Three Submissions/
├── W3-PM-FINAL - Cracking & Password Recovery - Mazen Ashraf.docx / .pdf
├── W3-PM2-Task2-hash-calculator.png
├── W3-PM2-Task5-6-password-cracker-crack.png
└── W3-PM1-Task3-onlinehashcrack-pdf-hash-extractor.png
```

---

## 🔐 Security & Ethical Use

All activities were performed against lab material provided for the internship
or against systems the author owns. Unauthorised access is a criminal offence.

---

## 🔗 Tools & Resources

- **John the Ripper:** https://www.openwall.com/john/
- **Johnny:** https://openwall.info/wiki/john/johnny
- **Networkwalks Hash Calculator:** https://networkwalks.com/hash-calculator/
- **Networkwalks Password Cracker:** https://networkwalks.com/password-cracker/

---

## 👤 Author

**Mazen Ashraf**
Cybersecurity Intern — Network Walks

LinkedIn: [your-linkedin-url]
GitHub: [your-github-username]

---

## 📌 Project Information

**Program:** Network Walks Cybersecurity Internship · **Week:** 03 ·
**Modules:** W3-PM1 (JTR) + W3-PM2 (Networkwalks Tools) ·
**Deliverable:** W3-PM-FINAL Penetration Testing / Cracking Report

© 2026 Mazen Ashraf. All rights reserved.
