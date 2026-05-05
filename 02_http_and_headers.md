# 🌐 HTTP & Headers Challenges

> **Kategoriya:** Web Exploitation — HTTP Protocol  
> **Challenges:** GET aHEAD · WebDecode  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [Anonymous]

---

## Umumiy tushuncha

HTTP (HyperText Transfer Protocol) — web saytlar bilan muloqot qilish uchun ishlatiladigan protokol. Brauzer faqat GET va POST so'rovlarini yuboradi, lekin HTTP'da boshqa metodlar ham mavjud: `HEAD`, `OPTIONS`, `PUT`, `DELETE` va hokazo. Ba'zan flag yoki maxfiy ma'lumot HTTP response headerlarida yashirilgan bo'ladi.

---

## 1. GET aHEAD

### 📋 Challenge tavsifi
Challenge nomi o'zidayoq hint beradi: `GET` va `HEAD` — HTTP metodlari. Flag oddiy GET so'rovi bilan emas, balki `HEAD` metodi yordamida response headerlarini tekshirish orqali topiladi.

### 🔍 Yechim jarayoni

**1-qadam — Saytni brauzerda ochamiz:**  
Oddiy sahifa ko'rinadi. Hech qanday flag yo'q.

**2-qadam — HTTP HEAD so'rovini yuboramiz:**  
`HEAD` metodi GET kabi ishlaydi, lekin response body qaytarmaydi — faqat headerlar.

**`curl` bilan:**
```bash
curl -I http://mercury.picoctf.net:XXXXX/
# -I flagi HEAD so'rovini yuboradi
```

yoki aniq ko'rsatish uchun:
```bash
curl -X HEAD http://mercury.picoctf.net:XXXXX/ -v
```

**3-qadam — Response headerlarini ko'ramiz:**
```http
HTTP/1.1 200 OK
Content-Type: text/html
flag: picoCTF{hEadS_uP_iT5_cOmInG_785d4d6a}
X-Powered-By: PHP/7.4
```

**4-qadam — Flag headerda! ✅**

**Alternativ — Burp Suite bilan:**
1. Burp Suite → Proxy → Intercept
2. So'rovni ushlab, metodini `GET` dan `HEAD` ga o'zgartiramiz
3. Forward bosamiz
4. Response headerlarini ko'ramiz

**Alternativ — DevTools bilan:**
```
F12 → Network → Sahifani yangilaymiz → 
Request tanlash → Headers bo'limi → Response Headers
```

### 🛠️ Ishlatilgan toollar
| Tool | Buyruq/Maqsad |
|------|---------------|
| `curl` | `curl -I http://site/` |
| Burp Suite | HTTP metod o'zgartirish |
| DevTools Network | Response headers ko'rish |

### 💡 Xulosa va o'rganilgan narsalar
> HTTP metodlari — `GET`, `POST`, `HEAD`, `OPTIONS`, `PUT`, `DELETE`. Brauzer odatda faqat GET/POST ishlatadi, lekin server boshqalarini ham qabul qilishi mumkin. Response headerlar ko'pincha muhim ma'lumotlar o'z ichiga oladi: server versiyasi, framework, cookie settings va h.k.

**Kalit tushunchalar:** HTTP methods, HEAD request, response headers, `curl -I`

---

## 2. WebDecode

### 📋 Challenge tavsifi
Flag encoded holda — ya'ni o'zgartirilgan ko'rinishda — saytda yashirilgan. Uni topib, to'g'ri decode qilish kerak. Encoding turli shakllarda bo'lishi mumkin: Base64, URL encoding, HTML encoding va boshqalar.

### 🔍 Yechim jarayoni

**1-qadam — Saytni ochamiz va source kodni ko'ramiz:**
```
Ctrl + U   →   HTML source kodi
```

**2-qadam — Encoded qiymatni topamiz:**  
HTML'da g'alati ko'rinishdagi matn yoki attribute qiymatini ko'ramiz:
```html
<p hidden>cGljb0NURntXZWJEZWNvZGVfdzNiXzE2Mzg3YX0=</p>
```
yoki:
```html
<div data-info="picoCTF%7BWeb%44ecode%7D"></div>
```

**3-qadam — Encoding turini aniqlaymiz:**

| Ko'rinish | Encoding turi |
|-----------|--------------|
| `=` bilan tugaydi, A-Z/a-z/0-9/+/ | Base64 |
| `%XX` ko'rinish | URL encoding |
| `&amp;`, `&#60;` ko'rinish | HTML entities |
| Faqat 0-9 va A-F | Hex |

**4-qadam — Decode qilamiz:**

**Base64 uchun:**
```bash
# Terminal:
echo "cGljb0NURntXZWJEZWNvZGVfdzNiXzE2Mzg3YX0=" | base64 -d
# Natija: picoCTF{WebDecode_w3b_16387a}

# Browser Console:
atob("cGljb0NURntXZWJEZWNvZGVfdzNiXzE2Mzg3YX0=")
```

**URL encoding uchun:**
```bash
# Terminal (Python):
python3 -c "from urllib.parse import unquote; print(unquote('picoCTF%7BWeb%44ecode%7D'))"
# Natija: picoCTF{WebDecode}

# Browser Console:
decodeURIComponent("picoCTF%7BWeb%44ecode%7D")
```

**CyberChef bilan (universal):**  
[gchq.github.io/CyberChef](https://gchq.github.io/CyberChef) → "Magic" operatsiyasi har qanday encoding'ni avtomatik aniqlaydi.

**5-qadam — Flag topildi! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `base64 -d` | Base64 decode (Linux) |
| `atob()` | Base64 decode (Browser) |
| `decodeURIComponent()` | URL decode (Browser) |
| [CyberChef](https://gchq.github.io/CyberChef) | Universal decoder |

### 💡 Xulosa va o'rganilgan narsalar
> Encoding — bu shifrlash emas! Base64, URL encoding, HTML entities — bularning barchasi ochiq standartlar bo'lib, har kim decode qila oladi. Maxfiy ma'lumotlar hech qachon faqat encoding bilan himoya qilinmasligi kerak.

**Encoding vs Encryption farqi:**
```
Encoding  → Har kim decode qila oladi (Base64, URL, HTML)
Encryption → Kalit bo'lmasa decode qilib bo'lmaydi (AES, RSA)
```

**Kalit tushunchalar:** Base64, URL encoding, HTML entities, CyberChef, encoding vs encryption

---

## 📊 Umumiy xulosa

HTTP protokolini chuqur tushunish web xavfsizlikning asosidir:

```
HTTP Request anatomy:
┌─────────────────────────────┐
│ GET /page HTTP/1.1          │ ← Metod + yo'l + versiya
│ Host: example.com           │ ← Headerlar
│ Cookie: session=abc123      │
│                             │
│ (body — GET'da odatda yo'q) │
└─────────────────────────────┘

HTTP Response anatomy:
┌─────────────────────────────┐
│ HTTP/1.1 200 OK             │ ← Status
│ Content-Type: text/html     │ ← Headerlar (flag bu yerda!)
│ flag: picoCTF{...}          │
│                             │
│ <html>...</html>            │ ← Body (flag bu yerda ham!)
└─────────────────────────────┘
```

### Keyingi qadamlar
- Burp Suite bilan HTTP request/response interception
- `curl` buyrug'ini chuqur o'rganish
- HTTP status kodlarini o'rganish (200, 301, 403, 404, 500...)

---

*PicoCTF | Web Exploitation | HTTP & Headers*
