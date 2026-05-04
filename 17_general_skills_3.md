# ⚙️ General Skills — Batch 3

> **Kategoriya:** General Skills  
> **Challenges:** Nice netcat... · Verify · Scan Surprise · Flag Hunters  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. Nice netcat...

### 📋 Challenge tavsifi
Netcat orqali serverga ulaniladi. Server raqamlar ketma-ketligini chiqaradi — ular ASCII kodlari. Ularni harflarga o'girib flag topiladi.

### 🔍 Yechim jarayoni

**1-qadam — Netcat bilan ulanish:**
```bash
nc mercury.picoctf.net PORT
# 112 105 99 111 67 84 70 123 103 48 48 100 95 110 101 116 99 52 116 95 99 ...
```

**2-qadam — ASCII raqamlarini decode qilamiz:**
```python
numbers = [112,105,99,111,67,84,70,123,103,48,48,100,95,110,
           101,116,99,52,116,95,99,48,109,109,52,110,100,125]

flag = ''.join(chr(n) for n in numbers)
print(flag)
# picoCTF{g00d_netc4t_c0mm4nd}
```

**Yoki bir qatorda:**
```bash
nc mercury.picoctf.net PORT | python3 -c "
import sys
nums = sys.stdin.read().split()
print(''.join(chr(int(n)) for n in nums))
"
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `nc` | Serverga ulanish |
| Python `chr()` | ASCII → harf |

### 💡 Xulosa
> ASCII — har bir belgi 0-127 oralig'idagi raqam. `chr(112)` = `'p'`. Netcat chiqishini Python bilan pipe qilib avtomatik decode qilish mumkin.

**Kalit tushunchalar:** Netcat, ASCII decode, chr(), pipe

---

## 2. Verify

### 📋 Challenge tavsifi
Fayl beriladi. Faylning SHA256 hash'i tekshiriladi — to'g'ri fayl ekanligini verify qilish va flag topish kerak.

### 🔍 Yechim jarayoni

**1-qadam — SSH bilan ulanish:**
```bash
ssh ctf-player@saturn.picoctf.net -p PORT
```

**2-qadam — Fayllarni ko'ramiz:**
```bash
ls
# checksum.txt  files/
cat checksum.txt
# 3ad37c4b6ccb546a3598b9b1fd0e0452d7571986d935ec8adacd0c89a4a9f9d6
```

**3-qadam — To'g'ri faylni topamiz:**
```bash
# files/ papkasidagi barcha fayllarning SHA256 ni tekshiramiz:
cd files/
sha256sum * | grep "3ad37c4b6ccb546a3598b9b1fd0e0452d7571986d935ec8adacd0c89a4a9f9d6"
# 3ad37c4... fileName.txt

cat fileName.txt | base64 -d
# picoCTF{...}
```

**Avtomatlashtirish:**
```bash
checksum=$(cat checksum.txt)
for f in files/*; do
    if sha256sum "$f" | grep -q "$checksum"; then
        echo "Found: $f"
        cat "$f" | base64 -d
    fi
done
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `sha256sum` | Hash hisoblash |
| `grep` | Hash moslikni topish |
| `base64 -d` | Decode qilish |

### 💡 Xulosa
> SHA256 — kriptografik hash funksiya. Faylning yaxlitligini (integrity) tekshirishda ishlatiladi. Bir xil fayl — har doim bir xil hash.

```bash
sha256sum file.txt      # SHA256 hash
md5sum file.txt         # MD5 hash
sha1sum file.txt        # SHA1 hash
```

**Kalit tushunchalar:** SHA256, file integrity, hash verification, bash loop

---

## 3. Scan Surprise

### 📋 Challenge tavsifi
QR kod beriladi. Uni scan qilish yoki decode qilish orqali flag topiladi.

### 🔍 Yechim jarayoni

**1-qadam — SSH orqali QR kodni yuklaymiz:**
```bash
ssh ctf-player@atlas.picoctf.net -p PORT
ls
# flag.png (QR kod)
```

**2-qadam — Faylni local'ga ko'chiramiz:**
```bash
# Local terminalda:
scp -P PORT ctf-player@atlas.picoctf.net:~/flag.png .
```

**3-qadam — QR kodni decode qilamiz:**
```bash
# zbar tool bilan:
zbarimg flag.png
# QR-Code: picoCTF{...}

# Python bilan:
pip install pyzbar pillow
python3 -c "
from pyzbar.pyzbar import decode
from PIL import Image
result = decode(Image.open('flag.png'))
print(result[0].data.decode())
"
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `zbarimg` | QR kod decode |
| `scp` | Fayl ko'chirish |
| Python `pyzbar` | QR decode library |
| Online: [zxing.org](https://zxing.org/w/decode.jspx) | Online QR decoder |

### 💡 Xulosa
> QR kod — ikki o'lchamli barcode. `zbarimg` Linux'da QR va boshqa barcodelarni decode qiladi. `scp` — SSH orqali fayl ko'chirish.

```bash
scp -P PORT user@host:~/file.txt ./  # Remote → Local
scp -P PORT ./file.txt user@host:~/  # Local → Remote
```

**Kalit tushunchalar:** QR code, zbarimg, scp, pyzbar

---

## 4. Flag Hunters

### 📋 Challenge tavsifi
Netcat orqali serverga ulaniladi. Server interaktiv muloqot o'tkazadi — to'g'ri javob berib flag olinadi. Ba'zan source kod tahlil qilinadi.

### 🔍 Yechim jarayoni

**1-qadam — Serverga ulanish:**
```bash
nc mimas.picoctf.net PORT
```

**2-qadam — Source kodini yuklab ko'ramiz:**
```bash
# Agar source berilsa:
cat source.py
# ...
# if user_input in flag:
#     print("You found part of the flag!")
```

**3-qadam — Flag qismlarini topamiz:**
```bash
# Server javoblarini kuzatib, to'g'ri so'zlarni topamiz
# yoki source'dan flag'ni o'qiymiz:
grep -i "flag\|picoCTF" source.py
# picoCTF{...}
```
✅ Flag topildi!

### 💡 Xulosa
> Interaktiv server challengelar — Python socket yoki pwntools bilan avtomatlashtirish mumkin.

```python
from pwn import *
conn = remote('mimas.picoctf.net', PORT)
print(conn.recvall().decode())
```

**Kalit tushunchalar:** Netcat, interactive shell, pwntools, source analysis

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy tool |
|-----------|---------|-------------|
| Nice netcat... | ASCII decode | `nc` + Python `chr()` |
| Verify | SHA256 verification | `sha256sum` + bash loop |
| Scan Surprise | QR decode | `zbarimg`, `scp` |
| Flag Hunters | Interactive netcat | `nc`, pwntools |

---

*PicoCTF | General Skills | Batch 3*
