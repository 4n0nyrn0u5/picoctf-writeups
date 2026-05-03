# 🔬 Forensics — Batch 2

> **Kategoriya:** Forensics  
> **Challenges:** Obedient Cat · strings it · Glory of the Garden · Static ain't always noise · information · CanYouSee  
> **Daraja:** Beginner  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. Obedient Cat

### 📋 Challenge tavsifi
Fayl beriladi — uni `cat` buyrug'i bilan o'qish kifoya.

### 🔍 Yechim jarayoni
```bash
cat flag
# picoCTF{s4n1ty_v3r1f1c4ti0n_...}
```
✅ Flag topildi!

### 💡 Xulosa
> `cat` — eng oddiy fayl o'qish buyrug'i. Ba'zan CTF'da eng oddiy yechim to'g'ri yechim.

**Kalit tushunchalar:** cat, file reading, Linux basics

---

## 2. strings it

### 📋 Challenge tavsifi
Binary faylda flag matn sifatida yashirilgan. `strings` buyrug'i binary'dan o'qilishi mumkin bo'lgan matnlarni chiqaradi.

### 🔍 Yechim jarayoni
```bash
strings strings | grep "picoCTF"
# picoCTF{5tRIng5_1T_...}
```
✅ Flag topildi!

### 💡 Xulosa
> `strings` — binary fayl ichidagi o'qilishi mumkin bo'lgan matnlarni chiqaradi. Forensics va reverse engineering'da birinchi qadamlardan biri.

```bash
strings file              # Barcha stringlar
strings file | grep flag  # Filtrlash
strings -n 8 file         # Min 8 belgili stringlar
```

**Kalit tushunchalar:** strings command, binary analysis, text extraction

---

## 3. Glory of the Garden

### 📋 Challenge tavsifi
Rasm fayli beriladi. Flag rasmning metadata yoki binary tarkibida yashirilgan.

### 🔍 Yechim jarayoni
```bash
# 1-usul — strings:
strings garden.jpg | grep "picoCTF"
# picoCTF{more_than_m33ts_the_3y3...}

# 2-usul — hexdump:
xxd garden.jpg | grep -a "picoCTF"

# 3-usul — exiftool:
exiftool garden.jpg
```
✅ Flag topildi!

### 💡 Xulosa
> Rasm fayllari (JPEG, PNG) oxiriga qo'shimcha ma'lumot qo'shish mumkin — bu JPEG'ning tuzilishi tufayli. `strings` bu ma'lumotni ko'rsatadi.

**Kalit tushunchalar:** Image forensics, strings, JPEG structure, file appending

---

## 4. Static ain't always noise

### 📋 Challenge tavsifi
Binary fayl va bash skript beriladi. Skriptni ishga tushirib yoki binary'ni tahlil qilib flag topiladi.

### 🔍 Yechim jarayoni
```bash
# Bash skriptni ko'ramiz:
cat ltdis.sh
# #!/bin/bash
# strings "$1"

# Binary'ga qo'llaymiz:
bash ltdis.sh static
# Ko'p matn...

bash ltdis.sh static | grep "picoCTF"
# picoCTF{d1s4bl3_tH3_n0t1f1c4t10n5_...}
```
✅ Flag topildi!

### 💡 Xulosa
> ELF binary (Linux executable) ichida ko'p matn bo'lishi mumkin — satrlar, xato xabarlari, va yashirilgan flaglar. `strings` + `grep` kombinatsiyasi tez topib beradi.

**Kalit tushunchalar:** ELF binary, bash scripting, strings, static analysis

---

## 5. information

### 📋 Challenge tavsifi
Rasm faylining metadata'sida (EXIF) flag yashirilgan.

### 🔍 Yechim jarayoni
```bash
exiftool cat.jpg
# ExifTool Version Number: 12.16
# File Name: cat.jpg
# ...
# License: cGljb0NURntleGlmX21ldGFkYXRhX...
```

**License qiymatini Base64 decode qilamiz:**
```bash
echo "cGljb0NURntleGlmX21ldGFkYXRhX..." | base64 -d
# picoCTF{exif_metadata_...}
```
✅ Flag topildi!

### 💡 Xulosa
> EXIF metadata — rasm haqida ma'lumot (kamera, GPS, sana). Forensics'da muhim artefakt. Real hayotda rasmlardagi GPS koordinatalar joylashuvni aniqlashda ishlatiladi.

```bash
exiftool image.jpg          # Barcha metadata
exiftool -GPS* image.jpg    # Faqat GPS
mat2 image.jpg              # Metadata tozalash
```

**Kalit tushunchalar:** EXIF metadata, exiftool, Base64, image forensics

---

## 6. CanYouSee

### 📋 Challenge tavsifi
Rasm beriladi. Flag ko'rinmaydi, lekin metadata'da yashirilgan.

### 🔍 Yechim jarayoni
```bash
exiftool ukn_reality.jpg
# Attribution URL: cGljb0NURntPcGVuX...

echo "cGljb0NURntPcGVuX..." | base64 -d
# picoCTF{Open_...}
```
✅ Flag topildi!

**Alternativ — barcha usullar:**
```bash
strings ukn_reality.jpg | grep picoCTF
xxd ukn_reality.jpg | grep -a pico
binwalk ukn_reality.jpg
steghide extract -sf ukn_reality.jpg
```

### 💡 Xulosa
> Rasm forensics uchun standart workflow:
```bash
1. file image.jpg       # Fayl turi
2. exiftool image.jpg   # Metadata
3. strings image.jpg    # Matn
4. binwalk image.jpg    # Ichki fayllar
5. steghide extract     # Stego
6. zsteg image.png      # LSB (PNG)
```

**Kalit tushunchalar:** EXIF, exiftool, Base64, image metadata forensics

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy tool |
|-----------|---------|-------------|
| Obedient Cat | cat | `cat flag` |
| strings it | Binary strings | `strings \| grep` |
| Glory of the Garden | Image strings | `strings image.jpg` |
| Static ain't always noise | ELF analysis | `strings + grep` |
| information | EXIF metadata | `exiftool` |
| CanYouSee | EXIF + Base64 | `exiftool` + `base64 -d` |

### Forensics Tezkor Workflow
```bash
file <fayl>              # 1. Tur aniqlash
cat <fayl>               # 2. To'g'ridan o'qish
strings <fayl> | grep picoCTF  # 3. Matn qidirish
exiftool <fayl>          # 4. Metadata
xxd <fayl> | head        # 5. Hex ko'rish
binwalk <fayl>           # 6. Ichki fayllar
steghide extract -sf <fayl>    # 7. Stego
```

---

*PicoCTF | Forensics | Batch 2*
