# 🔬 Forensics Challenges

> **Kategoriya:** Forensics  
> **Challenges:** Hidden in plainsight · DISKO 1 · Corrupted file · Flag in Flame  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## Umumiy tushuncha

Forensics — raqamli ma'lumotlarni tahlil qilib yashirilgan ma'lumotlarni topish ilmi. Fayl tuzilishi, metadata, steganography va binary tahlil asosiy texnikalar hisoblanadi.

---

## 1. Hidden in plainsight

### 📋 Challenge tavsifi
Flag ko'z oldida — lekin ko'rinmayapti. Matnda, rasmda yoki faylda yashirilgan ma'lumot bor. Ko'pincha whitespace, Unicode yoki LSB steganography ishlatiladi.

### 🔍 Yechim jarayoni

**1-qadam — Faylni ko'ramiz:**
```bash
file hidden.txt        # Fayl turi
cat hidden.txt         # Matnni ko'rish
xxd hidden.txt | head  # Hex ko'rish
```

**2-qadam — Ko'rinmas belgilarni topamiz:**
```bash
# Bo'sh joylar va unicode belgilarni ko'rish:
cat -A hidden.txt           # $ va ^ belgilar ko'rinadi
strings hidden.txt          # Matn qatorlarini chiqarish
hexdump -C hidden.txt       # To'liq hex dump
```

**3-qadam — Steghide bilan yashirilgan ma'lumot:**
```bash
steghide extract -sf image.jpg
# Parol so'rasa: bo'sh Enter

# Alternativ:
binwalk hidden_file         # Ichidagi fayllarni topish
binwalk -e hidden_file      # Chiqarib olish
```

**4-qadam — Whitespace steganography:**
```bash
# Har bir bo'sh joy — bit ifodalashi mumkin
# stegsnow tool:
stegsnow -C hidden.txt
```

**5-qadam — Flag topildi! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `strings` | Binary'dan matn |
| `xxd` / `hexdump` | Hex ko'rish |
| `steghide` | Stego extraction |
| `binwalk` | Fayl ichidagi fayllar |
| `cat -A` | Ko'rinmas belgilar |

### 💡 Xulosa
> "Hidden in plain sight" — ma'lumot ko'z oldida, lekin ko'rinmas. Whitespace, Unicode zero-width characters, va LSB (Least Significant Bit) steganography eng keng tarqalgan usullar.

**Kalit tushunchalar:** Steganography, whitespace encoding, `strings`, `binwalk`

---

## 2. DISKO 1

### 📋 Challenge tavsifi
Disk image (`.img` yoki `.dd`) fayli beriladi. Disk ichidagi fayl tizimini mount qilib, yashirilgan flagni topish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Fayl turini aniqlaymiz:**
```bash
file disko.img
# disko.img: Linux rev 1.0 ext4 filesystem data
```

**2-qadam — Disk image'ni mount qilamiz:**
```bash
# Mount point yaratamiz:
mkdir /tmp/disko

# Mount qilamiz:
sudo mount -o loop disko.img /tmp/disko
cd /tmp/disko
ls -la
```

**3-qadam — Flag qidiramiz:**
```bash
find /tmp/disko -name "flag*" 2>/dev/null
find /tmp/disko -name "*.txt" 2>/dev/null
grep -r "picoCTF{" /tmp/disko 2>/dev/null
```

**4-qadam — Yashirin fayllar:**
```bash
ls -la /tmp/disko/          # Yashirin fayllar (. bilan boshlanadiganlar)
ls -la /tmp/disko/...       # Barcha subdirektoriyalar
```

**5-qadam — Unmount:**
```bash
sudo umount /tmp/disko
```

✅ Flag topildi!

**Alternativ — foremost bilan deleted fayllar:**
```bash
foremost -i disko.img -o output/
# O'chirilgan fayllar ham recovery qilinadi
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `file` | Fayl turi aniqlash |
| `mount -o loop` | Disk image mount |
| `find` | Fayl qidirish |
| `foremost` | Deleted file recovery |
| `autopsy` | GUI forensics tool |

### 💡 Xulosa
> Disk forensics — real hayotda incident response'ning muhim qismi. Mount qilish, yashirin fayllar va o'chirilgan fayllarni topish — professional forensics mutaxassisining asosiy ko'nikmalari.

**Kalit tushunchalar:** Disk image, mount, ext4, file recovery, `foremost`

---

## 3. Corrupted file

### 📋 Challenge tavsifi
Fayl "buzilgan" — to'g'ridan-to'g'ri ochib bo'lmaydi. Fayl signaturasi (magic bytes) xato yoki noto'g'ri. Hex editor yordamida to'g'rilab, faylni ochish kerak.

### 🔧 File Signatures (Magic Bytes)
```
PNG:  89 50 4E 47 0D 0A 1A 0A    (\x89PNG\r\n)
JPEG: FF D8 FF E0                 (ÿØÿà)
PDF:  25 50 44 46                 (%PDF)
ZIP:  50 4B 03 04                 (PK)
GIF:  47 49 46 38 39 61           (GIF89a)
ELF:  7F 45 4C 46                 (\x7fELF)
```

### 🔍 Yechim jarayoni

**1-qadam — Faylni tekshiramiz:**
```bash
file corrupted_file
# corrupted_file: data  ← tur aniqlanmadi, buzilgan!

xxd corrupted_file | head -5
# 00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
# Lekin birinchi byte noto'g'ri bo'lishi mumkin
```

**2-qadam — Hex editor bilan o'zgartiramiz:**
```bash
# hexedit bilan:
hexedit corrupted_file
# Birinchi byteni to'g'ri signature bilan almashtiramiz

# Python bilan:
with open('corrupted_file', 'rb') as f:
    data = bytearray(f.read())

# PNG signature to'g'rilash:
data[0:8] = bytes([0x89, 0x50, 0x4E, 0x47, 0x0D, 0x0A, 0x1A, 0x0A])

with open('fixed_file.png', 'wb') as f:
    f.write(data)
```

**3-qadam — To'g'rilangan faylni ochamiz:**
```bash
file fixed_file.png
# fixed_file.png: PNG image data  ← Endi to'g'ri!
eog fixed_file.png  # Rasmni ko'rish
```

**4-qadam — Rasmdagi flag ko'rinadi! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `xxd` | Hex ko'rish |
| `hexedit` | Hex editor |
| Python `bytearray` | Bytes o'zgartirish |
| `file` | Fayl turi tekshirish |
| [hexed.it](https://hexed.it) | Online hex editor |

### 💡 Xulosa
> Har bir fayl turi maxsus "signature" (magic bytes) bilan boshlanadi. CTF'larda bu bytlar ataylab o'zgartiriladi. `xxd` bilan hex ko'rib, to'g'ri signature bilan almashtirish — forensics'ning asosiy texnikasi.

**Kalit tushunchalar:** Magic bytes, file signature, hex editing, file carving

---

## 4. Flag in Flame

### 📋 Challenge tavsifi
Flag rasm yoki video'da yashirilgan — olovga o'xshash vizual effect ichida. Steganography yoki frame extraction texnikasi kerak.

### 🔍 Yechim jarayoni

**Rasm bo'lsa:**

**1-qadam — Rasmni tahlil qilamiz:**
```bash
file flame.png
exiftool flame.png          # Metadata
strings flame.png           # Matn qatorlari
zsteg flame.png             # LSB steganography (PNG)
steghide extract -sf flame.jpg  # JPEG stego
```

**2-qadam — Rang kanallarini tekshiramiz (stegsolve):**
```bash
# stegsolve GUI da:
# Red 0, Green 0, Blue 0 → LSB planes ko'rish
java -jar stegsolve.jar
# File → Open → flame.png
# "<" va ">" bilan rang tekshiramiz
```

**3-qadam — Alpha channel:**
```python
from PIL import Image
img = Image.open('flame.png').convert('RGBA')
# Alpha kanalini ajratib ko'rish:
r, g, b, a = img.split()
a.save('alpha.png')
```

**Video bo'lsa:**

```bash
# Framelarni chiqarish:
ffmpeg -i flame.mp4 frames/frame%04d.png

# Har bir frameni tekshirish:
for f in frames/*.png; do
    strings "$f" | grep -i "picoctf"
done
```

✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `zsteg` | PNG LSB steganography |
| `steghide` | JPEG steganography |
| `stegsolve` | Visual stego analysis |
| `exiftool` | Image metadata |
| `ffmpeg` | Video frame extraction |
| Python PIL | Image manipulation |

### 💡 Xulosa
> Rasm steganography'da flag ko'pincha LSB (Least Significant Bit) ga yashiriladi — har bir pixel'ning oxirgi biti. Ko'z bilan farqlab bo'lmaydi, lekin `zsteg` va `stegsolve` aniqlaydi.

**Kalit tushunchalar:** LSB steganography, color channels, `zsteg`, `stegsolve`, frame extraction

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy tool |
|-----------|---------|-------------|
| Hidden in plainsight | Whitespace/Unicode stego | `strings`, `binwalk` |
| DISKO 1 | Disk image forensics | `mount`, `foremost` |
| Corrupted file | Magic bytes repair | `xxd`, `hexedit` |
| Flag in Flame | Image/Video stego | `zsteg`, `stegsolve` |

### Forensics Workflow
```bash
# Har qanday fayl uchun:
1. file <fayl>           # Tur aniqlash
2. exiftool <fayl>       # Metadata
3. strings <fayl>        # Matn qidirish
4. xxd <fayl> | head     # Hex ko'rish
5. binwalk <fayl>        # Ichidagi fayllar
6. steghide extract      # Stego tekshirish
```

---

*PicoCTF | Forensics*
