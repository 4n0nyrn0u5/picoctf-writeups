# 🔬 Forensics & OSINT Challenges

> **Kategoriya:** Forensics · OSINT · Web Recon  
> **Challenges:** StegoRSA · Scavenger Hunt · Log Hunt  
> **Daraja:** Intermediate  

---

## Umumiy tushuncha

Forensics — raqamli ma'lumotlarni tahlil qilib yashirilgan ma'lumotlarni topish ilmi. OSINT (Open Source Intelligence) — ochiq manbalardan ma'lumot yig'ish. Bu kategoriyada logik fikrlash, toollarni bilish va sabr-toqat muhim rol o'ynaydi.

---

## 1. StegoRSA

### 📋 Challenge tavsifi
Ikkita texnikani birlashtiradi: **Steganography** (ma'lumotni rasmda yashirish) va **RSA** (kriptografik algoritm). Rasm ichiga RSA bilan shifrlangan ma'lumot yashirilgan, biz uni topib, RSA kalitini topib, decrypt qilishimiz kerak.

### 🔧 Asosiy tushunchalar

**Steganography** — ma'lumotni boshqa ma'lumot ichiga yashirish san'ati. Rasm faylining har bir pixel'ining oxirgi bitiga ma'lumot yashirish mumkin (LSB — Least Significant Bit steganography).

**RSA** — ochiq kalit kriptografiyasi. `n = p × q` (p va q katta tub sonlar). Agar `n` kichik bo'lsa yoki `p` va `q` topilsa, RSA buzilishi mumkin.

### 🔍 Yechim jarayoni

**1-qadam — Rasmdan yashirilgan ma'lumotni chiqaramiz:**

```bash
# steghide bilan:
steghide extract -sf image.jpg
# Parol so'rasa, bo'sh Enter bosing yoki oddiy parollar sinab ko'ring

# zsteg bilan (PNG uchun):
zsteg image.png

# stegsolve (GUI tool):
java -jar stegsolve.jar
```

Natijada RSA parametrlari chiqadi:
```
n = 1522605027922533360535618378132637429718068114961380688657908494580122963258952897654000350692006139
e = 65537
c = 13418798...(ciphertext)
```

**2-qadam — RSA parametrlarini tahlil qilamiz:**

Agar `n` kichik bo'lsa, uni faktorlashtiramiz:

```bash
# factordb.com orqali (online):
# n ni kiritib, p va q ni topamiz

# Python bilan (kichik n uchun):
python3 -c "
from sympy import factorint
n = 1522605027922533360535618378132637429718068114961380688657908494580122963258952897654000350692006139
print(factorint(n))
"
```

yoki [factordb.com](http://factordb.com) saytiga `n` ni kiritamiz.

**3-qadam — RSA private key yaratamiz:**

```python
from Crypto.Util.number import inverse

# Topilgan qiymatlar:
p = 37975227936943673922808872755445627854565536638199
q = 40094690950920881030683735292761468389214899724061
e = 65537
n = p * q
c = 13418798...  # ciphertext

# Private key hisoblash:
phi = (p - 1) * (q - 1)
d = inverse(e, phi)

# Decrypt:
m = pow(c, d, n)

# Sonni matnга o'girish:
flag = m.to_bytes((m.bit_length() + 7) // 8, 'big').decode()
print(flag)
# picoCTF{...}
```

**4-qadam — Flag! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad | O'rnatish |
|------|--------|-----------|
| `steghide` | JPEG/BMP stego extract | `apt install steghide` |
| `zsteg` | PNG stego extract | `gem install zsteg` |
| `stegsolve` | GUI stego analyzer | [jar yuklab olish](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install) |
| [factordb.com](http://factordb.com) | RSA `n` faktorlashtirish | Online |
| Python `pycryptodome` | RSA hisoblash | `pip install pycryptodome` |

### 💡 Xulosa va o'rganilgan narsalar
> **RSA xavfsizligi** `n` ni faktorlashtirish qiyinligiga asoslanadi. Agar `p` va `q` kichik, yaqin yoki ma'lum bo'lsa — RSA buzilishi mumkin. Real hayotda 2048-bit va undan katta kalit uzunliklari ishlatiladi.

> **Steganography** — forensics CTF'larning klassik qismi. Rasmni ko'rish bilan "normal" ko'rinadi, lekin ichida ma'lumot yashiringan bo'lishi mumkin.

**Kalit tushunchalar:** LSB steganography, RSA factorization, private key recovery, `steghide`, `zsteg`

---

## 2. Scavenger Hunt

### 📋 Challenge tavsifi
"Xazina qidirish o'yini" — bir necha turli joylarda yashirilgan flag bo'laklarini topish kerak. Har bir bo'lak keyingi bo'lakka hint beradi. Web recon, file inspection va OSINT texnikaları kerak.

### 🔍 Yechim jarayoni

**1-qadam — Saytni ochamiz va source kodni ko'ramiz:**
```
Ctrl + U
```
```html
<!-- Part 1: picoCTF{t -->
```

**2-qadam — CSS faylini ko'ramiz:**
```
F12 → Sources → style.css
```
```css
/* Part 2: h4ts_4_l0 */
```

**3-qadam — JS faylini ko'ramiz:**
```javascript
/* Part 3: t_0f_pl4c */
```

**4-qadam — robots.txt:**
```
/robots.txt
```
```
# Part 4: 3s_2b0ef
Disallow: /secret-page
```

**5-qadam — `.htaccess` faylini tekshiramiz:**  
Apache serverlarda `.htaccess` konfiguratsiya fayli bo'lishi mumkin:
```
http://site/.htaccess
```
```
# Part 5: _f4a5}
```

**6-qadam — Barcha qismlarni birlashtiramiz:**
```
picoCTF{t h4ts_4_l0 t_0f_pl4c 3s_2b0ef _f4a5}
→ picoCTF{th4ts_4_l0t_0f_pl4c3s_2b0ef_f4a5}
```
✅ Flag tayyor!

### 📍 Tekshiriladigan joylar ro'yxati

```
/robots.txt          ← Har doim birinchi
/sitemap.xml         ← Sahifalar ro'yxati
/.htaccess           ← Apache config
/humans.txt          ← Dasturchilar haqida
/crossdomain.xml     ← Flash policy
/.git/               ← Git repository
/admin/              ← Admin panel
/backup/             ← Backup fayllar
/config.php          ← Konfiguratsiya
/changelog.txt       ← O'zgarishlar tarixi
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Browser | Manual recon |
| `curl` | Fayllarni olish |
| `gobuster` / `dirb` | Directory enumeration |
| DevTools | Source, CSS, JS tekshirish |

```bash
# gobuster bilan:
gobuster dir -u http://site/ \
  -w /usr/share/wordlists/dirb/common.txt

# dirb bilan:
dirb http://site/
```

### 💡 Xulosa va o'rganilgan narsalar
> Web saytning "ko'rinmas" joylari — `.htaccess`, `robots.txt`, `sitemap.xml`, `.git` — ko'pincha muhim ma'lumotlarni saqlaydi. Real pentest paytida bu joylarni tizimli tekshirish — reconnaissance (recon) bosqichining muhim qismi.

**Kalit tushunchalar:** Web recon methodology, hidden files, directory enumeration, `gobuster`

---

## 3. Log Hunt

### 📋 Challenge tavsifi
Server log fayli berilgan. Logni tahlil qilib, noto'g'ri yoki shubhali so'rovlarni topish, keyin esa flag ma'lumotini chiqarish kerak. Log analysis — forensics va incident response'ning asosiy qismi.

### 🔍 Yechim jarayoni

**1-qadam — Log faylini yuklab olamiz va ko'ramiz:**
```bash
cat access.log | head -50
```

Odatiy Apache/Nginx log formati:
```
192.168.1.1 - - [25/Apr/2025:12:34:56 +0000] "GET /page HTTP/1.1" 200 1234
IP - - [SANA:VAQT] "METOD YO'L PROTOKOL" STATUS BAYT
```

**2-qadam — Shubhali so'rovlarni qidiramiz:**

```bash
# 200 dan farqli status kodlarni ko'ramiz:
grep -v " 200 " access.log | head -20

# POST so'rovlarni ko'ramiz:
grep "POST" access.log

# Flag-ga o'xshash narsalarni qidiramiz:
grep -i "flag\|picoctf\|secret" access.log

# Specific fayllar so'rovini ko'ramiz:
grep "\.txt\|\.php\|admin\|config" access.log
```

**3-qadam — Base64 encoded URL'larni topamiz:**

Ba'zan flag URL'da encoded bo'ladi:
```bash
# URL decode qilish:
grep "%" access.log | python3 -c "
import sys
from urllib.parse import unquote
for line in sys.stdin:
    print(unquote(line.strip()))
"
```

**4-qadam — Eng ko'p so'rov yuborgan IP'ni topamiz:**
```bash
# IP statistikasi:
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10
```

**5-qadam — Flagni topamiz:**

Log faylda flag quyidagi joylarda bo'lishi mumkin:
- URL path'da: `GET /picoCTF{...} HTTP/1.1`
- User-Agent headerda
- Query string'da: `?search=picoCTF{...}`
- Base64 encoded qiymat sifatida

```bash
# Hammasini birga qidiramiz:
grep -oP 'picoCTF\{[^}]+\}' access.log
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Buyruq | Maqsad |
|------|--------|--------|
| `grep` | `grep "pattern" file` | Naqshni qidirish |
| `awk` | `awk '{print $1}'` | Ustun ajratish |
| `sort` + `uniq` | `sort \| uniq -c` | Statistika |
| `cut` | `cut -d' ' -f1` | Maydon ajratish |
| Python | `urllib.parse.unquote()` | URL decode |

**Foydali `grep` flaglari:**
```bash
grep -i   # Case-insensitive
grep -o   # Faqat moslikni chiqarish
grep -P   # Perl regex (kuchli)
grep -v   # Teskari (mos bo'lmaganlarni)
grep -n   # Qator raqamini ko'rsatish
grep -r   # Rekursiv qidirish
```

### 💡 Xulosa va o'rganilgan narsalar
> Log tahlili — incident response va forensics'ning asosi. Real hayotda log'lar SQL injection urinishlarini, brute force hujumlarni, va boshqa shubhali faoliyatni aniqlashga yordam beradi.

**Log tahlil metodologiyasi:**
```
1. Log formatini tushunish (Apache, Nginx, syslog...)
2. Anormalliklarni aniqlash (unusual status codes, IPs, paths)
3. Vaqt bo'yicha filtrlash (incident sodir bo'lgan vaqt)
4. Zararli pattern'larni qidirish (SQLi, LFI, XSS payloads)
5. Dalillarni yig'ish va tahlil qilish
```

**Kalit tushunchalar:** Log analysis, `grep`/`awk`, web server logs, pattern matching, forensics methodology

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy tool |
|-----------|---------|-------------|
| StegoRSA | Steganography + RSA decrypt | `steghide`, Python |
| Scavenger Hunt | Web recon + multi-source | Browser, `gobuster` |
| Log Hunt | Log file analysis | `grep`, `awk` |

### Forensics va OSINT uchun muhim tamoyillar
1. **Tizimli bo'ling** — har qadamni yozing
2. **Hech narsani e'tiborsiz qoldirmang** — kichik detail katta hint bo'lishi mumkin
3. **Toollarni biling** — to'g'ri tool vaqtni tejaydi
4. **Encoding'ni tekshiring** — Base64, hex, URL encoding doim tekshirilsin
5. **Metadata unutmang** — fayl metadata ham muhim ma'lumot saqlaydi

```bash
# Fayl metadata (EXIF):
exiftool image.jpg

# Fayl turi:
file suspicious_file

# Strings (binary'dan matn):
strings binary_file | grep -i flag
```

---

*PicoCTF | Forensics & OSINT*
