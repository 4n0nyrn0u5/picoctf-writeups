# 🚩 PicoCTF Writeups

> **Platform:** [PicoCTF](https://picoctf.org/)  
> **Kategoriya:** Web Exploitation · Cryptography · Forensics · General Skills · Reverse Engineering  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)  
> **Yangilangan:** 26.04.2026

Ushbu repository mening PicoCTF platformasida yechgan challengelarim uchun professional writeuplar to'plami.

---

## 📁 Fayl tuzilmasi

```
📦 picoctf-writeups
 ┣ 📄 README.md
 ┣ 📄 01_client_side_recon.md          → Inspect HTML, Includes, Insp3ct0r, Bookmarklet, Unminify, where are the robots
 ┣ 📄 02_http_and_headers.md           → GET aHEAD, WebDecode
 ┣ 📄 03_authentication_sessions.md    → Local Authority, dont-use-client-side, Cookie Monster, Old Sessions
 ┣ 📄 04_server_side_attacks.md        → Crack the Gate 1, SSTI1
 ┣ 📄 05_forensics_osint.md            → StegoRSA, Scavenger Hunt, Log Hunt
 ┣ 📄 06_cryptography.md               → interencdec, Mod 26, The Numbers, 13
 ┣ 📄 07_web_exploitation_medium.md    → 3v@l, Secrets
 ┣ 📄 08_general_skills.md             → FANTASY CTF, Super SSH, Binary Search, Time Machine, Commitment Issues, MultiCode, Password Profiler, Piece by Piece, Binary Digits, SUDO MAKE ME A SANDWICH
 ┣ 📄 09_web_command_injection.md      → ping-cmd
 ┣ 📄 10_forensics.md                  → Hidden in plainsight, DISKO 1, Corrupted file, Flag in Flame
 ┣ 📄 11_reverse_engineering.md        → bytemancy 0, bytemancy 1, Riddle Registry, Printer Shares
 ┣ 📄 12_general_skills_2.md           → what's a net cat?, Wave a flag, First Grep, Tab Tab Attack, runme.py, convertme.py, fixme1.py, fixme2.py, Magikarp Ground Mission, Collaborative Development, Blame Game, First Find, Big Zip
 ┣ 📄 13_cryptography_2.md             → Bases, Codebook, Transformation, repetitions, 2warm, binhexa, endianness
 ┣ 📄 14_web_exploitation_2.md         → logon, Cookies, Quizploit, Shared Secrets
 ┣ 📄 15_forensics_2.md                → Obedient Cat, strings it, Glory of the Garden, Static ain't always noise, information, CanYouSee
 ┗ 📄 16_reverse_engineering_2.md      → Warmed Up, Lets Warm Up, vault-door-training, PW Crack 1, PW Crack 2, HashingJobApp, Glitch Cat
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
| 24 | FANTASY CTF | General Skills | ⭐ | Netcat, interactive shell |
| 25 | Super SSH | General Skills | ⭐ | SSH connection |
| 26 | Binary Search | General Skills | ⭐⭐ | Binary search algorithm |
| 27 | Time Machine | General Skills | ⭐⭐ | Git log, commit history |
| 28 | Commitment Issues | General Skills | ⭐⭐ | Git checkout, forensics |
| 29 | MultiCode | General Skills | ⭐⭐ | Multi-layer encoding |
| 30 | Password Profiler | General Skills | ⭐⭐ | Wordlist generation, brute force |
| 31 | Piece by Piece | General Skills | ⭐ | File exploration |
| 32 | Binary Digits | General Skills | ⭐ | Binary to ASCII |
| 33 | SUDO MAKE ME A SANDWICH | General Skills | ⭐⭐⭐ | Privilege escalation, GTFOBins |
| 34 | ping-cmd | Web | ⭐⭐⭐ | OS Command Injection |
| 35 | Hidden in plainsight | Forensics | ⭐⭐ | Steganography, strings |
| 36 | DISKO 1 | Forensics | ⭐⭐ | Disk image forensics |
| 37 | Corrupted file | Forensics | ⭐⭐ | Magic bytes, hex editing |
| 38 | Flag in Flame | Forensics | ⭐⭐ | Image/Video steganography |
| 39 | bytemancy 0 | Reverse Eng. | ⭐⭐ | XOR decryption |
| 40 | bytemancy 1 | Reverse Eng. | ⭐⭐⭐ | Multi-layer reverse engineering |
| 41 | Riddle Registry | Reverse Eng. | ⭐⭐ | Windows Registry forensics |
| 42 | Printer Shares | Reverse Eng. | ⭐⭐ | SMB enumeration |
| 43 | what's a net cat? | General Skills | ⭐ | Netcat basics |
| 44 | Wave a flag | General Skills | ⭐ | Binary --help flag |
| 45 | First Grep | General Skills | ⭐ | grep pattern search |
| 46 | Tab, Tab, Attack | General Skills | ⭐ | Tab autocomplete |
| 47 | runme.py | General Skills | ⭐ | Python execution |
| 48 | convertme.py | General Skills | ⭐ | Base conversion |
| 49 | fixme1.py | General Skills | ⭐ | Python indentation fix |
| 50 | fixme2.py | General Skills | ⭐ | Python syntax fix |
| 51 | Magikarp Ground Mission | General Skills | ⭐⭐ | SSH + multi-part flag |
| 52 | Collaborative Development | General Skills | ⭐⭐ | Git branches |
| 53 | Blame Game | General Skills | ⭐⭐ | git blame, git log |
| 54 | First Find | General Skills | ⭐ | find command |
| 55 | Big Zip | General Skills | ⭐⭐ | unzip + grep -r |
| 56 | Bases | Cryptography | ⭐ | Base64/32/16 decode |
| 57 | Codebook | Cryptography | ⭐⭐ | Substitution cipher |
| 58 | Transformation | Cryptography | ⭐⭐ | Unicode packing |
| 59 | repetitions | Cryptography | ⭐⭐ | Multi-layer Base64 |
| 60 | 2warm | Cryptography | ⭐ | Decimal to Binary |
| 61 | binhexa | Cryptography | ⭐⭐ | Binary ↔ Hex |
| 62 | endianness | Cryptography | ⭐⭐ | Little/Big endian |
| 63 | logon | Web | ⭐⭐ | Cookie bool bypass |
| 64 | Cookies | Web | ⭐⭐ | IDOR, cookie enum |
| 65 | Quizploit | Web | ⭐⭐ | Client-side JS bypass |
| 66 | Shared Secrets | Web | ⭐⭐⭐ | XOR image cryptography |
| 67 | Obedient Cat | Forensics | ⭐ | cat command |
| 68 | strings it | Forensics | ⭐ | strings command |
| 69 | Glory of the Garden | Forensics | ⭐ | Image strings |
| 70 | Static ain't always noise | Forensics | ⭐⭐ | ELF binary analysis |
| 71 | information | Forensics | ⭐⭐ | EXIF metadata + Base64 |
| 72 | CanYouSee | Forensics | ⭐⭐ | EXIF metadata |
| 73 | Warmed Up | Reverse Eng. | ⭐ | Hex to Decimal |
| 74 | Lets Warm Up | Reverse Eng. | ⭐ | Hex to ASCII |
| 75 | vault-door-training | Reverse Eng. | ⭐ | Java source analysis |
| 76 | PW Crack 1 | Reverse Eng. | ⭐ | Hardcoded password |
| 77 | PW Crack 2 | Reverse Eng. | ⭐⭐ | MD5 hash cracking |
| 78 | HashingJobApp | Reverse Eng. | ⭐⭐ | MD5 hashing, automation |
| 79 | Glitch Cat | Reverse Eng. | ⭐⭐ | chr() obfuscation |

---

## 🛠️ Ishlatilgan toollar

- **Browser DevTools** — Inspection, Sources, Network, Application
- **Burp Suite** — HTTP interception va manipulation
- **curl** — HTTP request manipulation
- **jwt.io** — JWT decode/encode
- **CyberChef** — Encoding/decoding (Magic funksiya)
- **sqlmap** — SQL injection avtomatlashtirish
- **steghide / stegsolve / zsteg** — Steganography
- **Python** — Custom scripts, RSA decryption, hashlib, socket
- **gobuster / ffuf** — Directory enumeration
- **Nmap** — Port scanning
- **netcat (nc)** — Server ulanish, port testing
- **ssh** — Masofaviy ulanish
- **git** — Commit history, branches, blame tahlil
- **Ghidra** — Binary decompilation
- **smbclient / enum4linux** — SMB enumeration
- **binwalk / foremost** — File carving, disk forensics
- **xxd / hexedit** — Hex editing
- **hydra / CUPP** — Brute force, wordlist generation
- **exiftool** — Image metadata tahlil
- **john / hashcat** — Hash cracking
- **GTFOBins** — Privilege escalation reference
- **strings** — Binary'dan matn chiqarish
- **find / grep** — Fayl va matn qidirish

---

## 📚 Foydali resurslar

- [PicoCTF](https://picoctf.org/) — CTF platformasi
- [OWASP Top 10](https://owasp.org/Top10/) — Web xavfsizlik asoslari
- [PortSwigger Web Security Academy](https://portswigger.net/web-security) — Bepul labs
- [HackTricks](https://book.hacktricks.xyz/) — CTF va pentest reference
- [CyberChef](https://gchq.github.io/CyberChef/) — Encoding/decoding tool
- [dcode.fr](https://www.dcode.fr) — 500+ shifr decoder
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings) — Payloadlar to'plami
- [GTFOBins](https://gtfobins.github.io) — Linux privilege escalation
- [Ghidra](https://ghidra-sre.org) — Bepul NSA decompiler
- [revshells.com](https://www.revshells.com) — Reverse shell generator
- [crackstation.net](https://crackstation.net) — Online hash cracker
- [ExplainShell](https://explainshell.com) — Linux buyruqlarini tushuntirish

---

*Happy Hacking! 🔐*
