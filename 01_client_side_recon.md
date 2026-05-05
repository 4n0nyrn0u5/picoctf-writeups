# 🕵️ Client-Side Recon Challenges

> **Kategoriya:** Web Exploitation — Client Side  
> **Challenges:** Inspect HTML · Includes · Insp3ct0r · Bookmarklet · Unminify · where are the robots  
> **Daraja:** Beginner  
> **Muallif:** [Anonymous]
---

## Umumiy tushuncha

Client-side recon — bu web saytning foydalanuvchi brauzerida ochiq bo'lgan barcha ma'lumotlarini (HTML, CSS, JavaScript, robots.txt va boshqalar) sinchkovlik bilan o'rganish jarayoni. Ko'plab dasturchilar xato qilib, maxfiy ma'lumotlarni ushbu fayllarda qoldirib ketishadi.

---

## 1. Inspect HTML

### 📋 Challenge tavsifi
Flag HTML sahifaning source kodida — ko'pincha HTML comment ichida — yashirilgan. Sahifaning oddiy ko'rinishida hech narsa ko'rinmaydi, lekin source kodni ochsangiz flag topiladi.

### 🔍 Yechim jarayoni

**1-qadam — Saytni ochamiz:**  
Berilgan URL'ga o'tamiz. Oddiy sahifa ko'rinadi, flag hech qayerda ko'rinmaydi.

**2-qadam — HTML source kodini ko'ramiz:**  
```
Ctrl + U   →  View Page Source (barcha brauzerda ishlaydi)
```
yoki sichqonchaning o'ng tugmasi → "View Page Source".

**3-qadam — Flag izlaymiz:**  
```
Ctrl + F   →   picoCTF{   deb qidiramiz
```

HTML kodda quyidagicha comment ko'rinadi:
```html
<!-- picoCTF{1n5p3ct0r_0f_h7ml_1d390a55} -->
```

**4-qadam — Flag topildi! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `Ctrl+U` | HTML source code ko'rish |
| `Ctrl+F` | Sahifada qidirish |

### 💡 Xulosa va o'rganilgan narsalar
> HTML commentlar (`<!-- -->`) foydalanuvchi uchun ko'rinmaydi, lekin source kodda ochiq turadi. Dasturchilar ba'zan test parollarini, TODO izohlarini yoki maxfiy yo'llarni commentda qoldirib ketishadi — bu real hayotdagi katta xavfsizlik xatosi.

**Kalit tushunchalar:** HTML inspection, View Page Source, developer comments

---

## 2. Includes

### 📋 Challenge tavsifi
Flag sahifaning asosiy HTML faylida emas, balki unga ulangan tashqi CSS yoki JavaScript fayllarida yashirilgan. Tashqi resurslarni tekshirish kerak.

### 🔍 Yechim jarayoni

**1-qadam — DevTools ochamiz:**
```
F12   →   DevTools oynasi ochiladi
```

**2-qadam — Sources tabiga o'tamiz:**  
Sahifaga ulangan barcha fayllarni ko'ramiz:
- `style.css`
- `script.js`

**3-qadam — CSS faylni ochamiz:**  
```css
/* CSS fayl ichida: */
/* picoCTF{1ncluded_f1le5_ */
body {
    background: white;
}
```

**4-qadam — JS faylni ochamiz:**  
```javascript
// JS fayl ichida:
// _a4e8c8b8}
function greet() { console.log("Hello"); }
```

**5-qadam — Ikkala qismni birlashtiramiz:**
```
picoCTF{1nclu ded_f1le5_a4e8c8b8}
```
✅ Flag tayyor!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `F12` → Sources | Tashqi fayllarni ko'rish |
| `Ctrl+F` | Faylda qidirish |

### 💡 Xulosa va o'rganilgan narsalar
> Web sayt faqat `index.html` dan iborat emas. CSS, JS, image va boshqa resurslar ham tekshirilishi shart. Real pentestda bu fayllar API key, password, yoki maxfiy endpoint yo'llarini o'z ichiga olishi mumkin.

**Kalit tushunchalar:** External resources, CSS/JS inspection, multi-file flag

---

## 3. Insp3ct0r

### 📋 Challenge tavsifi
Nom "Inspector" degan ma'noni anglatadi. Flag uchta turli joyga bo'linib yashirilgan: HTML, CSS va JavaScript. Uchala qismni topib birlashtirish kerak.

### 🔍 Yechim jarayoni

**1-qadam — HTML'dan birinchi qism:**
```
Ctrl + U   →   source kodni ochamiz
```
```html
<!-- picoCTF{tru3_d3 -->
```

**2-qadam — CSS'dan ikkinchi qism:**  
`F12` → Elements → `<link rel="stylesheet">` tegini topamiz → CSS faylni ochamiz:
```css
/* t3ct1ve_0r_ju5t */
```

**3-qadam — JS'dan uchinchi qism:**  
Sources tab → `.js` faylni ochamiz:
```javascript
// _lucky?1f038553}
```

**4-qadam — Uchala qismni birlashtiramiz:**
```
picoCTF{tru3_d3t3ct1ve_0r_ju5t_lucky?1f038553}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `Ctrl+U` | HTML source |
| `F12` → Elements | CSS link topish |
| `F12` → Sources | JS fayl ko'rish |

### 💡 Xulosa va o'rganilgan narsalar
> Haqiqiy security audit paytida ham yashirilgan ma'lumotlar bir nechta fayllarga tarqatilgan bo'lishi mumkin. Barchasini tizimli tekshirish muhim. DevTools — web xavfsizlik mutaxassisining asosiy quroli.

**Kalit tushunchalar:** Multi-source investigation, comprehensive DevTools usage

---

## 4. Bookmarklet

### 📋 Challenge tavsifi
Sahifada JavaScript "bookmarklet" kodi berilgan. Uni brauzer address barida bajarish orqali flag ochiladi. `atob()` funksiyasi yordamida Base64 encoded flag decode qilinadi.

### 🔍 Yechim jarayoni

**1-qadam — Sahifadagi kodni ko'ramiz:**
```javascript
javascript:void(
  document.getElementById('flag').innerText = 
  atob('cGljb0NURntib29rbWFya2xldF8xMWY3NjV9')
);
```

**2-qadam — Kodni address barga joylashtiramiz:**  
URL satriga yuqoridagi kodni yozib Enter bosamiz.

**Alternativ — Console orqali:**
```javascript
// F12 → Console tabida:
atob('cGljb0NURntib29rbWFya2xldF8xMWY3NjV9')
// Natija: picoCTF{bookmarklet_11f765}
```

**3-qadam — Flag paydo bo'ldi! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Browser address bar | JS kodni bajarish |
| `F12` → Console | `atob()` funksiyasini ishlatish |
| CyberChef | Base64 decode (alternativ) |

### 💡 Xulosa va o'rganilgan narsalar
> `atob()` — brauzerning o'rnatilgan Base64 decode funksiyasi. Base64 bu **encoding** (shifrlash emas!) — har kim decode qila oladi. Shuning uchun Base64 ni xavfsizlik uchun ishlatish noto'g'ri.

**Kalit tushunchalar:** JavaScript execution, Base64 decoding, `atob()` function

---

## 5. Unminify

### 📋 Challenge tavsifi
Saytning JavaScript kodi "minify" qilingan — ya'ni barcha bo'shliqlar, yangi qatorlar va kommentlar olib tashlangan, o'zgaruvchilar qisqa nomlar bilan almashtirilgan. Flag shu o'qilishi qiyin kodda yashiringan.

### 🔍 Yechim jarayoni

**1-qadam — Minified JS faylni topamiz:**  
`F12` → Sources → `.js` fayl  

Minified ko'rinish:
```javascript
function a(b){return b.split('').reverse().join('')}var c=a('picoCTF{...}');document.write(c);
```

**2-qadam — Kodni formatlashtiramiz (Pretty Print):**  
DevTools Sources oynasining pastki qismidagi `{ }` tugmasini bosamiz.

**Alternativ — Online tool:**  
[beautifier.io](https://beautifier.io) → kodni joylashtiring → "Beautify" bosing.

**3-qadam — Formatlangan kodda qidiramiz:**
```
Ctrl + F   →   picoCTF{   yoki   flag
```

**4-qadam — Flag topildi! ✅**

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| DevTools `{ }` tugmasi | Pretty Print |
| [beautifier.io](https://beautifier.io) | Online formatter |
| [prettier.io](https://prettier.io/playground/) | Alternativ formatter |

### 💡 Xulosa va o'rganilgan narsalar
> Minification — performance uchun, xavfsizlik uchun emas. Minified kodni ham o'qish mumkin, faqat bir qadam qo'shimcha talab etiladi. Haqiqiy xavfsizlik faqat server-side da ta'minlanishi mumkin.

**Kalit tushunchalar:** JS minification/beautification, Pretty Print, code obfuscation

---

## 6. where are the robots

### 📋 Challenge tavsifi
`robots.txt` — web saytlarda search engine crawlerlariga qaysi sahifalarni indekslamaslikni aytadigan standart fayl. Lekin bu fayl public bo'ladi va undagi "yashirin" yo'llar hamma uchun ko'rinadi!

### 🔍 Yechim jarayoni

**1-qadam — robots.txt faylini ochamiz:**  
Sayt URL'iga `/robots.txt` qo'shamiz:
```
http://challenge-site.picoctf.net/robots.txt
```

**2-qadam — Faylni o'qiymiz:**
```
User-agent: *
Disallow: /1bb4c.html
```

**3-qadam — Disallow qilingan yo'lga o'tamiz:**
```
http://challenge-site.picoctf.net/1bb4c.html
```

**4-qadam — Bu sahifada flag yozilgan! ✅**
```
picoCTF{where_are_the_r0bots_a4a8e6c9}
```

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Browser | Manual URL manipulation |
| `curl` | `curl http://site/robots.txt` |

### 💡 Xulosa va o'rganilgan narsalar
> `robots.txt` — web reconnaissance'ning birinchi qadamlaridan biri. Real pentestda bu fayl admin panel, backup fayllari, va yashirin API endpointlarini ochib berishi mumkin. "Disallow" qilish — yashirish emas!

**Kalit tushunchalar:** robots.txt, web reconnaissance, directory enumeration

---

## 📊 Umumiy xulosa

Bu 6 ta challenge bitta asosiy tamoyilni o'rgatadi:

> **"Security through obscurity is not security"**  
> Ma'lumotni ko'zdan yashirish — bu xavfsizlik emas. HTML'da, CSS'da, JS'da yoki robots.txt'da saqlangan har qanday narsa — **public** hisoblanadi.

### Hujum vektori xaritasi
```
Brauzer
  ├── View Source (Ctrl+U)     → HTML comments
  ├── DevTools Sources (F12)   → CSS, JS fayllar
  ├── DevTools Application     → Cookies, Storage
  └── URL manipulation         → /robots.txt, /admin, /backup
```

---

*PicoCTF | Web Exploitation | Client-Side Recon*
