# 🚩 PicoCTF Writeups

> **By:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)  
> **Platform:** [PicoCTF](https://picoctf.org)  
> **Category:** Web Exploitation · Forensics · OSINT

---

## About

This repository contains my personal writeups for PicoCTF challenges. Each writeup includes a full step-by-step solution, tools used, and key takeaways to reinforce learning.

---

## 📁 Structure

```
picoctf-writeups/
├── 01_client_side_recon.md       → Inspect HTML, Includes, Insp3ct0r, Bookmarklet, Unminify, where are the robots
├── 02_http_and_headers.md        → GET aHEAD, WebDecode
├── 03_authentication_sessions.md → Local Authority, dont-use-client-side, Cookie Monster, Old Sessions
├── 04_server_side_attacks.md     → Crack the Gate 1 (SQLi), SSTI1
└── 05_forensics_osint.md         → StegoRSA, Scavenger Hunt, Log Hunt
```

---

## 📊 Challenges

| # | Challenge | Category | Difficulty | Technique |
|---|-----------|----------|------------|-----------|
| 1 | Inspect HTML | Web | ⭐ | HTML source inspection |
| 2 | Includes | Web | ⭐ | CSS/JS file inspection |
| 3 | Insp3ct0r | Web | ⭐ | Multi-source DevTools |
| 4 | Bookmarklet | Web | ⭐ | JS execution, Base64 |
| 5 | Unminify | Web | ⭐⭐ | JS beautification |
| 6 | where are the robots | Web | ⭐ | robots.txt recon |
| 7 | GET aHEAD | Web | ⭐⭐ | HTTP methods |
| 8 | WebDecode | Web | ⭐⭐ | Base64 / URL decode |
| 9 | Local Authority | Web | ⭐⭐ | Client-side auth bypass |
| 10 | dont-use-client-side | Web | ⭐⭐ | JS logic bypass |
| 11 | Cookie Monster Secret Recipe | Web | ⭐⭐ | Cookie manipulation |
| 12 | Old Sessions | Web | ⭐⭐⭐ | JWT exploitation |
| 13 | Crack the Gate 1 | Web | ⭐⭐⭐ | SQL Injection |
| 14 | SSTI1 | Web | ⭐⭐⭐ | Server-Side Template Injection |
| 15 | StegoRSA | Forensics | ⭐⭐⭐ | Steganography + RSA |
| 16 | Scavenger Hunt | OSINT | ⭐⭐ | Multi-source recon |
| 17 | Log Hunt | Forensics | ⭐⭐ | Log file analysis |

---

## 🛠️ Tools Used

![Burp Suite](https://img.shields.io/badge/Burp_Suite-FF6633?style=flat&logo=burp-suite&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)
![JWT](https://img.shields.io/badge/JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white)

- **Browser DevTools** — Inspection, Sources, Network, Application tabs
- **curl** — HTTP request manipulation
- **jwt.io** — JWT decode/encode
- **CyberChef** — Encoding/decoding
- **steghide / zsteg** — Steganography extraction
- **Python** — RSA decryption, custom scripts

---

## 🔗 Connect

- 🌐 GitHub: [4n0nyrn0u5](https://github.com/4n0nyrn0u5)
- 🏆 PicoCTF: [4n0nyrn0u5](https://play.picoctf.org)

---

> *"The quieter you become, the more you are able to hear."*  
> — Kali Linux

