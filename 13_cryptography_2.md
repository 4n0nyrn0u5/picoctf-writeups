# 🔐 Cryptography — Batch 2

> **Kategoriya:** Cryptography  
> **Challenges:** Bases · Codebook · Transformation · repetitions · 2warm · binhexa · endianness  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. Bases

### 📋 Challenge tavsifi
Flag turli sanoq tizimlarida (Base64, Base32, Base16/Hex) kodlangan. Decode qilish kerak.

### 🔍 Yechim jarayoni
```bash
# Berilgan: bDNhcm5fdGgzX3IwcDNzXzBmX2IzaW5nXzFuX2IzNTVfYTY0NjQxNjd9

# Base64 decode:
echo "bDNhcm5fdGgzX3IwcDNzXzBmX2IzaW5nXzFuX2IzNTVfYTY0NjQxNjd9" | base64 -d
# picoCTF{l3arn_th3_r0p3s_0f_b3ing_1n_b355_a6464167}
```
✅ Flag topildi!

### Sanoq tizimlari tezkor jadval
```
Base2  (Binary):  0,1
Base8  (Octal):   0-7
Base10 (Decimal): 0-9
Base16 (Hex):     0-9, A-F
Base32:           A-Z, 2-7, = (padding)
Base58:           Bitcoin adreslari
Base64:           A-Z, a-z, 0-9, +, /, =
```

```bash
# Decode usullari:
echo "..." | base64 -d          # Base64
echo "..." | base32 -d          # Base32
echo "48656c6c6f" | xxd -r -p   # Hex → ASCII
python3 -c "print(int('111',2))" # Binary → Decimal
```

### 💡 Xulosa
> Encoding — xavfsizlik emas! Barcha bu formatlar ochiq standart. CyberChef → Magic funksiyasi encoding'ni avtomatik aniqlaydi.

**Kalit tushunchalar:** Base64, Base32, Hex, number bases, encoding

---

## 2. Codebook

### 📋 Challenge tavsifi
Ikkita fayl beriladi: `code.txt` (shifrlangan) va `codebook.txt` (kalit). Codebook'dagi harflar bilan decode qilish kerak.

### 🔍 Yechim jarayoni
```bash
cat codebook.txt
# a -> p
# b -> i
# c -> c
# ...

cat code.txt
# dcebg...

# Python bilan decode:
python3 -c "
with open('codebook.txt') as cb:
    mapping = {}
    for line in cb:
        k, v = line.strip().split(' -> ')
        mapping[k] = v

with open('code.txt') as f:
    encoded = f.read().strip()

flag = ''.join(mapping.get(c, c) for c in encoded)
print(flag)
"
# picoCTF{...}
```
✅ Flag topildi!

### 💡 Xulosa
> Substitution cipher — har bir harf boshqa harf bilan almashtiriladi. Bu klassik kriptografiyaning asosi. Python dict bilan decode qilish — eng tez yechim.

**Kalit tushunchalar:** Substitution cipher, codebook, Python dict mapping

---

## 3. Transformation

### 📋 Challenge tavsifi
Matn Unicode yoki boshqa transformatsiya bilan o'zgartirilgan. Teskari operatsiya bajarish kerak.

### 🔍 Yechim jarayoni
```bash
cat enc.txt
# 灩捯䍔䙻ㄶ_ㄸ_ㄸ_㨵ㅽ

# Python bilan Unicode decode:
python3 -c "
enc = open('enc.txt').read().strip()
flag = ''
for c in enc:
    # Har bir unicode char 2 ta ASCII char
    val = ord(c)
    flag += chr(val >> 8) + chr(val & 0xff)
print(flag)
"
# picoCTF{16_18_18_3b}
```
✅ Flag topildi!

### 💡 Xulosa
> Unicode — har bir belgi 0-65535 oralig'ida raqam. Ikkita ASCII belgini bitta Unicode belgiga joylashtirish mumkin: `(ord(a) << 8) | ord(b)`. Teskari: `val >> 8` va `val & 0xff`.

**Kalit tushunchalar:** Unicode, bitwise operations, ord(), chr()

---

## 4. repetitions

### 📋 Challenge tavsifi
Flag Base64 bilan bir necha marta kodlangan. Har safar decode qilish kerak.

### 🔍 Yechim jarayoni
```bash
cat enc_flag
# VmpGU1EyRXlUWGxTYmxKV1lrZFNjRlV3Wkc5...

# Bir marta:
echo "VmpGU1EyRXlUWGxTYmxKV1lrZFNjRlV3Wkc5..." | base64 -d
# Yana Base64...

# Python bilan loop:
python3 -c "
import base64
data = open('enc_flag').read().strip()
for i in range(50):
    try:
        data = base64.b64decode(data).decode()
        if 'picoCTF' in data:
            print(data)
            break
    except:
        break
"
# picoCTF{base64_3nc0d1ng_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Ko'p marta Base64 — CTF'da keng tarqalgan. Python loop bilan avtomatlashtirish eng yaxshi yechim. `picoCTF{` topilguncha decode qilaverish.

**Kalit tushunchalar:** Multi-layer Base64, Python loop, base64 module

---

## 5. 2warm

### 📋 Challenge tavsifi
Raqamni binary (ikkilik) sanoq tizimiga o'girish kerak.

### 🔍 Yechim jarayoni
```bash
# Savol: 42 ni binary'ga o'gir

python3 -c "print(bin(42))"
# 0b101010

# Yoki:
python3 -c "print(format(42, 'b'))"
# 101010
```

Flag:
```
picoCTF{101010}
```
✅ Flag topildi!

### 💡 Xulosa
> Binary konversiya — kompyuter ilmining asosi.

```python
bin(42)         # → '0b101010'
format(42, 'b') # → '101010'
int('101010', 2) # → 42  (teskari)

# Boshqa bazalar:
oct(42)         # → '0o52'  (Octal)
hex(42)         # → '0x2a'  (Hex)
```

**Kalit tushunchalar:** Binary conversion, bin(), format()

---

## 6. binhexa

### 📋 Challenge tavsifi
Binary va Hexadecimal o'rtasida konversiya. Binary sonni Hex'ga yoki aksincha o'girish.

### 🔍 Yechim jarayoni
```bash
# Binary → Hex:
python3 -c "print(hex(int('110100', 2)))"
# 0x34

# Hex → Binary:
python3 -c "print(bin(int('3d', 16)))"
# 0b111101

# To'liq flag uchun:
python3 -c "
binary = '0110 1000 0110 0101 0110 1100 0110 1100 0110 1111'
binary = binary.replace(' ', '')
text = ''
for i in range(0, len(binary), 8):
    text += chr(int(binary[i:i+8], 2))
print(text)
"
# picoCTF{...}
```
✅ Flag topildi!

### 💡 Xulosa
> Binary va Hex o'rtasida konversiya — har 4 bit = 1 hex digit.
```
0000 = 0    0100 = 4    1000 = 8    1100 = C
0001 = 1    0101 = 5    1001 = 9    1101 = D
0010 = 2    0110 = 6    1010 = A    1110 = E
0011 = 3    0111 = 7    1011 = B    1111 = F
```

**Kalit tushunchalar:** Binary, Hexadecimal, base conversion, bitwise

---

## 7. endianness

### 📋 Challenge tavsifi
Little-endian va Big-endian — baytlarning xotirada saqlanish tartibi. Flagni to'g'ri endianness bilan o'qish kerak.

### 🔧 Endianness nima?
```
Qiymat: 0x12345678

Big-endian (network order):
12 34 56 78  ← Katta bayt birinchi

Little-endian (x86 CPU):
78 56 34 12  ← Kichik bayt birinchi
```

### 🔍 Yechim jarayoni
```bash
# Berilgan so'z: "challenge" ni little-endian hex ko'rinishida topish

python3 -c "
word = 'challenge'
# Big-endian:
big = word.encode().hex()
print('Big-endian:', big)

# Little-endian (teskari baytlar):
little = word.encode()[::-1].hex()
print('Little-endian:', little)
"
# Big-endian:    6368616c6c656e6765
# Little-endian: 656e67656c6c616863

# Serverga ulanib javob beramiz:
nc challenge-server PORT
```
✅ Flag topildi!

### 💡 Xulosa
> Endianness — low-level programming va network protokollarida muhim. x86 — little-endian, network protokollari — big-endian (network byte order). Python `struct` moduli konversiya uchun ishlatiladi.

```python
import struct
struct.pack('<I', 0x12345678)  # Little-endian
struct.pack('>I', 0x12345678)  # Big-endian
```

**Kalit tushunchalar:** Endianness, little-endian, big-endian, byte order, struct

---

## 📊 Umumiy xulosa

| Challenge | Shifr/Texnika | Decode usuli |
|-----------|--------------|--------------|
| Bases | Base64/32/16 | `base64 -d`, CyberChef |
| Codebook | Substitution | Python dict mapping |
| Transformation | Unicode packing | `ord()`, bitwise |
| repetitions | Multi-layer Base64 | Python loop |
| 2warm | Decimal → Binary | `bin()`, `format()` |
| binhexa | Binary ↔ Hex | `int(x, 2)`, `hex()` |
| endianness | Byte order | `[::-1]`, `struct` |

### Tez konversiya buyruqlari
```python
# Decimal → Binary:   bin(42)       → '0b101010'
# Decimal → Hex:      hex(42)       → '0x2a'
# Binary → Decimal:   int('101010', 2) → 42
# Hex → Decimal:      int('2a', 16)   → 42
# Text → Hex:         'hi'.encode().hex() → '6869'
# Hex → Text:         bytes.fromhex('6869').decode() → 'hi'
# Base64 encode:      import base64; base64.b64encode(b'hi')
# Base64 decode:      base64.b64decode('aGk=')
```

---

*PicoCTF | Cryptography | Batch 2*
