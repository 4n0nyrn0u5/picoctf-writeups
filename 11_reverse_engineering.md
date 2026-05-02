# 🔄 Reverse Engineering Challenges

> **Kategoriya:** Reverse Engineering  
> **Challenges:** bytemancy 0 · bytemancy 1 · Riddle Registry · Printer Shares  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## Umumiy tushuncha

Reverse Engineering — dasturni manba kodisiz tahlil qilish. Binary fayllarni, assembly kodini va protokollarni o'rganib, dastur qanday ishlashini tushunish va flagni topish.

---

## 1. bytemancy 0

### 📋 Challenge tavsifi
Binary fayl beriladi. Bayt operatsiyalari (XOR, AND, OR, shift) orqali shifrlangan flag. Dastur ichidagi mantiqni tushunib, teskari operatsiya bajarish kerak.

### 🔧 Bit operatsiyalari
```python
# XOR:
0b1010 ^ 0b1100 = 0b0110
A ^ key = B  →  B ^ key = A  (XOR teskari!)

# AND:
0b1010 & 0b1100 = 0b1000

# OR:
0b1010 | 0b1100 = 0b1110

# Shift:
0b1010 << 1 = 0b10100  (chapga shift)
0b1010 >> 1 = 0b0101   (o'ngga shift)
```

### 🔍 Yechim jarayoni

**1-qadam — Binary faylni tekshiramiz:**
```bash
file bytemancy0
strings bytemancy0         # Matn qatorlari
./bytemancy0               # Ishga tushiramiz
```

**2-qadam — Ghidra yoki objdump bilan disassemble:**
```bash
# objdump:
objdump -d bytemancy0 | less

# strings bilan encoded flagni topamiz:
strings bytemancy0 | grep -i "ctf\|flag\|pico"
```

**3-qadam — Python bilan teskari operatsiya:**
```python
# Agar XOR bilan shifrlangan bo'lsa:
encoded = [0x15, 0x3a, 0x2f, ...]  # stringdan olindi
key = 0x42

flag = ''.join(chr(b ^ key) for b in encoded)
print(flag)
# picoCTF{...}
```

**4-qadam — CyberChef bilan:**
```
CyberChef → XOR → Key: 0x42 → From Hex → Bake
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `strings` | Binary'dan matn |
| `objdump -d` | Disassembly |
| Ghidra | GUI decompiler (bepul) |
| Python | XOR/bit operatsiyalar |
| CyberChef | Visual decode |

### 💡 Xulosa
> XOR — eng ko'p ishlatiladigan shifrlash operatsiyasi, chunki teskari operatsiya ham XOR (kalit bir xil). `A XOR key = B` bo'lsa, `B XOR key = A`. Bu CTF'larda klassik pattern.

**Kalit tushunchalar:** XOR cipher, bit manipulation, `strings`, binary analysis

---

## 2. bytemancy 1

### 📋 Challenge tavsifi
bytemancy 0 dan murakkabrok — ko'p qavatli bit operatsiyalar va ehtimol turli kalit bilan. Dastur logikasini chuqurroq tahlil qilish kerak.

### 🔍 Yechim jarayoni

**1-qadam — bytemancy 0 dan farqni aniqlaymiz:**
```bash
diff <(strings bytemancy0) <(strings bytemancy1)
```

**2-qadam — Ghidra bilan decompile:**
```
Ghidra → New Project → Import bytemancy1 →
CodeBrowser → Analysis → Auto Analyze →
Functions → main() → Ko'rib chiqamiz
```

Decompiled kod taxminan:
```c
void main() {
    char flag[] = {0x15, 0x2a, ...};
    int key1 = 0x42;
    int key2 = 0x1f;
    
    for (int i = 0; i < len; i++) {
        flag[i] = (flag[i] ^ key1) + key2;
    }
}
```

**3-qadam — Teskari operatsiya:**
```python
encoded = [0x15, 0x2a, ...]
key1 = 0x42
key2 = 0x1f

# Teskari: avval key2 ni ayirib, keyin XOR
flag = ''.join(chr((b - key2) ^ key1) for b in encoded)
print(flag)
# picoCTF{...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Ghidra | Decompilation |
| `ltrace` | Library calls monitoring |
| `strace` | System calls monitoring |
| Python | Reverse operatsiyalar |
| GDB | Dynamic debugging |

```bash
# ltrace va strace:
ltrace ./bytemancy1    # Qaysi funksiyalar chaqiriladi
strace ./bytemancy1    # OS ga qaysi so'rovlar
```

### 💡 Xulosa
> Murakkab reverse engineering uchun Ghidra — eng yaxshi bepul tool. Decompiled C kodni o'qib, matematikani teskari bajarish asosiy ko'nikma. Operatsiyalar tartibi muhim: `(x ^ key1) + key2` uchun teskari `(y - key2) ^ key1`.

**Kalit tushunchalar:** Ghidra, decompilation, multi-layer XOR, reverse math

---

## 3. Riddle Registry

### 📋 Challenge tavsifi
Windows Registry yoki maxsus "registry" faylida flag yashirilgan. Registry — Windows'ning konfiguratsiya ma'lumotlar bazasi.

### 🔍 Yechim jarayoni

**Registry fayl berilsa:**

**1-qadam — Faylni tekshiramiz:**
```bash
file registry.reg
strings registry.reg | grep -i "flag\|ctf\|pico"
```

**2-qadam — Registry faylini o'qiymiz:**
```bash
cat registry.reg
# Windows Registry Editor Version 5.00
# [HKEY_LOCAL_MACHINE\SOFTWARE\picoCTF]
# "flag"="picoCTF{...}"
```

**Windows'da bo'lsa:**
```powershell
# regedit.exe → qidirish
reg query HKLM\SOFTWARE\picoCTF /v flag
```

**Linux'da Windows registry o'qish:**
```bash
# chntpw yoki reglookup:
reglookup registry.hiv | grep -i flag

# python-registry:
pip install python-registry
python3 -c "
from Registry import Registry
reg = Registry.Registry('registry.hiv')
key = reg.open('SOFTWARE\\\\picoCTF')
print(key.value('flag').value())
"
```
✅ Flag topildi!

**Netcat challenge bo'lsa:**
```bash
nc challenge-server PORT
# Riddle savollarga javob beramiz
# To'g'ri javoblar → flag chiqadi
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `strings` | Registry'dan matn |
| `reglookup` | Linux'da registry o'qish |
| `regedit` | Windows GUI |
| `reg query` | Windows CLI |
| python-registry | Python library |

### 💡 Xulosa
> Windows Registry — tizim va dastur sozlamalarini saqlaydi. Forensics'da registry — muhim artefakt: o'rnatilgan dasturlar, foydalanuvchi faoliyati, parollar, va boshqa ma'lumotlar. `HKCU\SOFTWARE` va `HKLM\SOFTWARE` — eng ko'p tekshiriladigan joylar.

**Kalit tushunchalar:** Windows Registry, `regedit`, `reg query`, forensics artefacts

---

## 4. Printer Shares

### 📋 Challenge tavsifi
SMB (Server Message Block) protokoli orqali ulashilgan printer yoki fayl papkasida flag yashirilgan. Network share'ga ulanib, fayllarni ko'rish kerak.

### 🔍 Yechim jarayoni

**1-qadam — SMB share'larni ro'yxatlaymiz:**
```bash
smbclient -L //challenge-server -N
# yoki
nmap --script smb-enum-shares -p 445 challenge-server
```

Natija:
```
Sharename    Type    Comment
---------    ----    -------
print$       Disk    Printer Drivers
PRINTER      Disk    
IPC$         IPC     IPC Service
```

**2-qadam — Share'ga kiramiz:**
```bash
smbclient //challenge-server/PRINTER -N
# smb: \> ls
# smb: \> get flag.txt
# smb: \> exit
```

**3-qadam — Yuklab olgan faylni o'qiymiz:**
```bash
cat flag.txt
# picoCTF{...}
```
✅ Flag topildi!

**enum4linux bilan to'liq enumeration:**
```bash
enum4linux -a challenge-server
# Users, shares, OS info, password policy
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `smbclient` | SMB share ulanish |
| `enum4linux` | SMB enumeration |
| `nmap --script smb-*` | SMB nmap scripts |
| `smbmap` | SMB permissions mapping |

```bash
# smbmap:
smbmap -H challenge-server
smbmap -H challenge-server -r PRINTER

# smbclient buyruqlari:
ls           # Fayllar ro'yxati
get file.txt # Fayl yuklab olish
mget *       # Barcha fayllar
cd folder    # Papkaga kirish
```

### 💡 Xulosa
> SMB — Windows tarmoqlarida fayl va printer ulashish uchun asosiy protokol. Noto'g'ri sozlangan SMB share'lar — real hayotda ham keng tarqalgan zaiflik. Anonymous access (`-N` flag) — autentifikatsiyasiz kirish imkoni.

**Kalit tushunchalar:** SMB, smbclient, network shares, anonymous access, enum4linux

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy tool |
|-----------|---------|-------------|
| bytemancy 0 | XOR decryption | Python, CyberChef |
| bytemancy 1 | Multi-layer reverse | Ghidra, Python |
| Riddle Registry | Registry forensics | `reglookup`, strings |
| Printer Shares | SMB enumeration | `smbclient`, `enum4linux` |

### Reverse Engineering Workflow
```bash
# Har qanday binary uchun:
1. file <binary>           # Tur aniqlash
2. strings <binary>        # Matn qatorlari
3. ltrace ./<binary>       # Library calls
4. strace ./<binary>       # System calls
5. objdump -d <binary>     # Disassembly
6. Ghidra                  # Decompilation
```

### Foydali resurslar
- [Ghidra](https://ghidra-sre.org) — Bepul NSA decompiler
- [IDA Free](https://hex-rays.com/ida-free/) — Industry standard
- [GTFOBins](https://gtfobins.github.io) — Binary exploitation reference
- [CyberChef](https://gchq.github.io/CyberChef) — XOR va boshqa operatsiyalar

---

*PicoCTF | Reverse Engineering*
