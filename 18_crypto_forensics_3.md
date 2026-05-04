# 🔐 Cryptography & Forensics — Batch 3

> **Kategoriya:** Cryptography · Forensics  
> **Challenges:** Transformation · hashcrack · Secret of the Polyglot  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. Transformation (v2)

### 📋 Challenge tavsifi
Matn Unicode transformation bilan shifrlangan. Har bir Unicode belgida ikkita ASCII belgi yashirilgan — ularni ajratib flag topiladi.

### 🔍 Yechim jarayoni

**1-qadam — Encoded matnni ko'ramiz:**
```bash
cat enc.txt
# 灩捯䍔䙻ㄶ_ㄸ_ㄸ_㨵ㅽ
```

**2-qadam — Python bilan decode:**
```python
enc = open('enc.txt', 'r').read().strip()
flag = ''
for c in enc:
    val = ord(c)
    # Yuqori 8 bit → birinchi belgi
    # Quyi 8 bit → ikkinchi belgi
    flag += chr(val >> 8) + chr(val & 0xff)
print(flag)
# picoCTF{16_18_18_3b}
```

**Bitwise operatsiyalar:**
```
ord('灩') = 0x7069 = 28777
0x7069 >> 8  = 0x70 = 112 = 'p'
0x7069 & 0xff = 0x69 = 105 = 'i'
```
✅ Flag topildi!

### 💡 Xulosa
> Unicode — 65536+ belgini qamrab oladi. Ikkita ASCII belgini (har biri 8 bit) bitta 16-bitli Unicode belgiga joylashtirish mumkin. `>>` (right shift) va `& 0xff` (AND mask) bilan ajratiladi.

**Kalit tushunchalar:** Unicode, bitwise shift, ord(), chr(), bit manipulation

---

## 2. hashcrack

### 📋 Challenge tavsifi
Hash beriladi — uni crack qilib asl parolni topish kerak. MD5, SHA1 yoki SHA256 bo'lishi mumkin.

### 🔍 Yechim jarayoni

**1-qadam — Hash turini aniqlaymiz:**
```bash
hash="482c811da5d5b4bc6d497ffa98491e38"
# Uzunligi 32 → MD5
# Uzunligi 40 → SHA1
# Uzunligi 64 → SHA256
```

**2-qadam — Online crack:**
```
crackstation.net → hash'ni kiritamiz
# password123
```

**3-qadam — john bilan:**
```bash
echo "482c811da5d5b4bc6d497ffa98491e38" > hash.txt
john hash.txt --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt
john hash.txt --show
# password123
```

**4-qadam — hashcat bilan:**
```bash
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
# -m 0   → MD5
# -m 100 → SHA1
# -m 1400 → SHA256
```

**5-qadam — Flag:**
```bash
nc challenge-server PORT
# Enter the password: password123
# picoCTF{...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| [crackstation.net](https://crackstation.net) | Online hash crack |
| `john` | Offline hash crack |
| `hashcat` | GPU accelerated crack |
| [hashes.com](https://hashes.com) | Alternativ online |

### Hash turlari va `-m` kodlari
```
MD5     → 32 belgi  → hashcat -m 0
SHA1    → 40 belgi  → hashcat -m 100
SHA256  → 64 belgi  → hashcat -m 1400
bcrypt  → $2b$...   → hashcat -m 3200
```

### 💡 Xulosa
> MD5 va SHA1 — zaif algoritmlar, rainbow table bilan tez cracklanadi. Parol saqlash uchun bcrypt, argon2, scrypt ishlatilish kerak. CrackStation 15 milliarddan ortiq hash'ni saqlaydi.

**Kalit tushunchalar:** Hash cracking, john, hashcat, rainbow table, MD5/SHA1

---

## 3. Secret of the Polyglot

### 📋 Challenge tavsifi
"Polyglot" fayl — bir vaqtning o'zida bir necha fayl formati sifatida o'qilishi mumkin bo'lgan fayl. Masalan, hem PNG, hem PDF bo'lishi mumkin. Har ikki formatda ham o'qib, flag bo'laklarini topish kerak.

### 🔧 Polyglot fayl nima?
```
Fayl boshi → PNG header (89 50 4E 47...)
Fayl o'rtasi → PDF ma'lumotlar (%PDF-1.4...)
→ Brauzerda ochilsa PDF, rasim ko'ruvchida PNG!
```

### 🔍 Yechim jarayoni

**1-qadam — Fayl turini aniqlaymiz:**
```bash
file flag2of2-final.pdf
# flag2of2-final.pdf: PNG image data + PDF document
```

**2-qadam — PDF sifatida ochamiz:**
```bash
# PDF viewer'da ochiladi → birinchi qism ko'rinadi:
# "1n_pn9_&_pdf_f0rm4t5}"
```

**3-qadam — PNG sifatida ochamiz:**
```bash
cp flag2of2-final.pdf flag.png
eog flag.png   # yoki any image viewer
# "picoCTF{f1u3n7_" ko'rinadi
```

**4-qadam — strings bilan:**
```bash
strings flag2of2-final.pdf | grep picoCTF
```

**5-qadam — Birlashtirish:**
```
picoCTF{f1u3n7_1n_pn9_&_pdf_f0rm4t5}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `file` | Fayl turi aniqlash |
| `strings` | Matn chiqarish |
| `xxd` | Hex ko'rish |
| `binwalk` | Ichki formatlar |
| Image viewer | PNG ko'rish |

```bash
binwalk flag2of2-final.pdf
# DECIMAL    HEX    DESCRIPTION
# 0          0x0    PNG image
# 512        0x200  PDF document
```

### 💡 Xulosa
> Polyglot fayllar — security bypass uchun ham ishlatiladi (masalan, rasm formatida fayl yuklash, lekin server PHP sifatida bajaradi). `file` buyrug'i magic bytes'ga qaraydi — kengaytmaga emas. Shuning uchun `.pdf` kengaytmali fayl PNG bo'lishi mumkin.

**Kalit tushunchalar:** Polyglot file, file format, magic bytes, binwalk, file carving

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Tool |
|-----------|---------|------|
| Transformation | Unicode bitwise decode | Python `ord()`, `>>`, `& 0xff` |
| hashcrack | MD5/SHA hash cracking | john, hashcat, crackstation |
| Secret of the Polyglot | Multi-format file analysis | `file`, `binwalk`, `strings` |

---

*PicoCTF | Cryptography & Forensics | Batch 3*
