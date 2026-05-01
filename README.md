# 🚩 PicoCTF Writeups

> **Platform:** [PicoCTF](https://picoctf.org/)  
> **Kategoriya:** Web Exploitation · Cryptography · Forensics · OSINT  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)  
> **Yangilangan:** 26.04.2026

Ushbu repository mening PicoCTF platformasida yechgan challengelarim uchun professional writeuplar to'plami.

---

## 📁 Fayl tuzilmasi

```
📦 picoctf-writeups
 ┣ 📄 README.md
 ┣ 📄 01_client_side_recon.md        → Inspect HTML, Includes, Insp3ct0r, Bookmarklet, Unminify, where are the robots
 ┣ 📄 02_http_and_headers.md         → GET aHEAD, WebDecode
 ┣ 📄 03_authentication_sessions.md  → Local Authority, dont-use-client-side, Cookie Monster, Old Sessions
 ┣ 📄 04_server_side_attacks.md      → Crack the Gate 1, SSTI1
 ┣ 📄 05_forensics_osint.md          → StegoRSA, Scavenger Hunt, Log Hunt
 ┣ 📄 06_cryptography.md             → interencdec, Mod 26, The Numbers, 13
 ┗ 📄 07_web_exploitation_medium.md  → 3v@l, Secrets
```

---

## 📊 Challenges jadvali

| # | Challenge | Kategoriya | Qiyinlik | Texnika |
|---|-----------|------------|----------|---------|
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
| 16 | Scavenger Hunt | OSINT/Web | ⭐⭐ | Multi-source recon |
| 17 | Log Hunt | Forensics | ⭐⭐ | Log file analysis |
| 18 | interencdec | Cryptography | ⭐⭐ | Base64 + ROT13 |
| 19 | Mod 26 | Cryptography | ⭐ | ROT13 |
| 20 | The Numbers | Cryptography | ⭐ | A1Z26 cipher |
| 21 | 13 | Cryptography | ⭐ | ROT13 |
| 22 | 3v@l | Web | ⭐⭐⭐ | eval() injection / RCE |
| 23 | Secrets | Web | ⭐⭐ | Hidden directory recon |

---

## 🛠️ Ishlatilgan toollar

- **Browser DevTools** — Inspection, Sources, Network, Application
- **Burp Suite** — HTTP interception va manipulation
- **curl** — HTTP request manipulation
- **jwt.io** — JWT decode/encode
- **CyberChef** — Encoding/decoding (Magic funksiya)
- **sqlmap** — SQL injection avtomatlashtirish
- **steghide / stegsolve / zsteg** — Steganography
- **Python** — Custom scripts, RSA decryption, A1Z26 decode
- **gobuster / ffuf** — Directory enumeration
- **Nmap** — Port scanning

---

## 📚 Foydali resurslar

- [PicoCTF](https://picoctf.org/) — CTF platformasi
- [OWASP Top 10](https://owasp.org/Top10/) — Web xavfsizlik asoslari
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) — Bepul labs
- [HackTricks](https://book.hacktricks.xyz/) — CTF va pentest reference
- [CyberChef](https://gchq.github.io/CyberChef/) — Encoding/decoding tool
- [dcode.fr](https://www.dcode.fr) — 500+ shifr decoder
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) — Payload'lar to'plami

---

*Happy Hacking! 🔐*
