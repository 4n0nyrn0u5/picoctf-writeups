# 🔄 Reverse Engineering — Batch 2

> **Kategoriya:** Reverse Engineering  
> **Challenges:** Warmed Up · Lets Warm Up · vault-door-training · PW Crack 1 · PW Crack 2 · HashingJobApp · Glitch Cat  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. Warmed Up

### 📋 Challenge tavsifi
Hex raqamni o'nlik sanoq tizimiga o'girish kerak.

### 🔍 Yechim jarayoni
```bash
# Savol: 0x3D (hex) = ? (decimal)

python3 -c "print(int('0x3D', 16))"
# 61

# Flag: picoCTF{61}
```
✅ Flag topildi!

### 💡 Xulosa
> Hex to Decimal konversiya — kompyuter ilmining asosi. `int(x, 16)` Python'da hex'dan o'nlikka.

```python
int('0x3D', 16)   # → 61
int('3D', 16)     # → 61  (0x prefikssiz ham ishlaydi)
0x3D              # → 61  (Python literal)
```

**Kalit tushunchalar:** Hexadecimal, decimal conversion, int()

---

## 2. Lets Warm Up

### 📋 Challenge tavsifi
Hex qiymatini ASCII harfga o'girish kerak.

### 🔍 Yechim jarayoni
```bash
# Savol: 0x70 = ?

python3 -c "print(chr(0x70))"
# p

# Flag: picoCTF{p}
```
✅ Flag topildi!

### 💡 Xulosa
> `chr()` — raqamdan ASCII belgiga. `ord()` — belgidan raqamga (teskari).

```python
chr(0x70)    # → 'p'
chr(112)     # → 'p'  (0x70 = 112)
ord('p')     # → 112
ord('A')     # → 65
```

**ASCII jadval:**
```
65=A  66=B  67=C  ...  90=Z
97=a  98=b  99=c  ... 122=z
48=0  49=1  50=2  ...  57=9
```

**Kalit tushunchalar:** ASCII, chr(), ord(), hex to char

---

## 3. vault-door-training

### 📋 Challenge tavsifi
Java source kodi beriladi. Kod ichida parol tekshiruvi bor — parolni topib flag olinadi.

### 🔍 Yechim jarayoni
```java
// VaultDoorTraining.java:
public boolean checkPassword(String password) {
    return password.equals("w4rm1ng_Up_w1tH_jAv4_....");
}
```

```bash
cat VaultDoorTraining.java | grep "equals\|password\|return"
# return password.equals("w4rm1ng_Up_w1tH_jAv4_....");
```

Flag: `picoCTF{w4rm1ng_Up_w1tH_jAv4_....}`
✅ Flag topildi!

### 💡 Xulosa
> Java source code'da `equals()` bilan parol tekshiruvi — eng oddiy hardcoded credential misoli. `grep` bilan tezda topiladi.

**Kalit tushunchalar:** Java source analysis, hardcoded credentials, grep

---

## 4. PW Crack 1

### 📋 Challenge tavsifi
Python skriptda parol hardcoded. Skriptni o'qib parolni topish kerak.

### 🔍 Yechim jarayoni
```bash
cat level1.py
# ...
# if( user_pw == "458ff61e"):
#     print("Correct password!")

# Parol topildi: 458ff61e
python3 level1.py
# Enter password: 458ff61e
# picoCTF{...}
```
✅ Flag topildi!

### 💡 Xulosa
> Hardcoded parol — xavfsizlikning eng katta xatolaridan biri. Source kodni o'qigan har kim parolni ko'rishi mumkin.

**Kalit tushunchalar:** Hardcoded password, source code analysis, Python

---

## 5. PW Crack 2

### 📋 Challenge tavsifi
Parol hash ko'rinishida saqlanган. Hashni crack qilish kerak.

### 🔍 Yechim jarayoni
```bash
cat level2.py
# import hashlib
# correct_pw_hash = "...md5 hash..."
# if hashlib.md5(user_pw.encode()).hexdigest() == correct_pw_hash:

# Hash'ni crackstation.net da sinab ko'ramiz:
# yoki john the ripper bilan:
echo "hash_value" > hash.txt
john hash.txt --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt

# Topilgan parol bilan:
python3 level2.py
# picoCTF{...}
```
✅ Flag topildi!

### 💡 Xulosa
> MD5 — zaif hash algoritmi. Rainbow table va wordlist orqali tez crack qilinadi. Parol saqlash uchun bcrypt, argon2 ishlatilishi kerak.

```bash
# Hash cracking toollar:
hashcat -m 0 hash.txt wordlist.txt  # MD5
john --format=raw-md5 hash.txt      # John the Ripper
# Online: crackstation.net, hashes.com
```

**Kalit tushunchalar:** MD5 hash, hash cracking, john, hashcat, crackstation

---

## 6. HashingJobApp

### 📋 Challenge tavsifi
Serverga ulanib, berilgan matnlarning hash'ini hisoblash kerak. Netcat orqali interaktiv muloqot.

### 🔍 Yechim jarayoni
```bash
nc saturn.picoctf.net PORT
# Please md5 hash the text between quotes: "hello"

python3 -c "import hashlib; print(hashlib.md5(b'hello').hexdigest())"
# 5d41402abc4b2a76b9719d911017c592

# Javob beramiz, ko'p marta takrorlanadi:
# Please md5 hash: "world"
python3 -c "import hashlib; print(hashlib.md5(b'world').hexdigest())"
# ...
# picoCTF{...}
```

**Avtomatlashtirish (Python):**
```python
import hashlib, socket, re

s = socket.socket()
s.connect(('saturn.picoctf.net', PORT))

while True:
    data = s.recv(1024).decode()
    print(data)
    match = re.search(r'"(.+?)"', data)
    if match:
        text = match.group(1)
        h = hashlib.md5(text.encode()).hexdigest()
        s.send((h + '\n').encode())
    if 'picoCTF' in data:
        break
```
✅ Flag topildi!

### 💡 Xulosa
> Hashing — bir tomonlama funksiya. Bir xil input har doim bir xil output beradi. Python `hashlib` moduli MD5, SHA1, SHA256 va boshqa algoritmlarni qo'llab-quvvatlaydi.

```python
import hashlib
hashlib.md5(b'text').hexdigest()     # MD5
hashlib.sha1(b'text').hexdigest()    # SHA1
hashlib.sha256(b'text').hexdigest()  # SHA256
```

**Kalit tushunchalar:** MD5 hashing, hashlib, Python socket, automation

---

## 7. Glitch Cat

### 📋 Challenge tavsifi
Netcat orqali serverga ulaniladi. Server Python f-string yoki eval() ishlatadi — injection orqali flag olinadi.

### 🔍 Yechim jarayoni
```bash
nc saturn.picoctf.net PORT
# Flag: picoCTF{gl1tch_m3_n0t_...} + obfuscated part

# Server chiqishini ko'ramiz:
# 'picoCTF{' + chr(0x74) + chr(0x68) + ...

# Python bilan decode:
python3 -c "print('picoCTF{' + chr(0x74) + chr(0x68) + chr(0x33) + ...)"
# picoCTF{gl1tch_m3_n0t_...}
```
✅ Flag topildi!

### 💡 Xulosa
> `chr(0x74)` = `chr(116)` = `'t'`. Obfuscated flag — harflar o'rniga hex kodlar ishlatilgan. Python REPL da to'g'ridan-to'g'ri baholash mumkin.

```python
chr(0x70)  # → 'p'
chr(0x69)  # → 'i'
chr(0x63)  # → 'c'
chr(0x6f)  # → 'o'
```

**Kalit tushunchalar:** chr(), hex obfuscation, netcat, Python eval

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy tool |
|-----------|---------|-------------|
| Warmed Up | Hex → Decimal | `int('0x3D', 16)` |
| Lets Warm Up | Hex → ASCII | `chr(0x70)` |
| vault-door-training | Java source read | `grep equals` |
| PW Crack 1 | Hardcoded password | Source code read |
| PW Crack 2 | MD5 hash crack | `john`, crackstation |
| HashingJobApp | MD5 hashing | `hashlib.md5()` |
| Glitch Cat | chr() decode | Python `chr()` |

### Tez konversiya
```python
int('3D', 16)          # Hex → Decimal: 61
chr(0x70)              # Hex → ASCII: 'p'
hex(65)                # Decimal → Hex: '0x41'
ord('A')               # ASCII → Decimal: 65
import hashlib
hashlib.md5(b'x').hexdigest()   # MD5 hash
hashlib.sha256(b'x').hexdigest() # SHA256
```

---

*PicoCTF | Reverse Engineering | Batch 2*
