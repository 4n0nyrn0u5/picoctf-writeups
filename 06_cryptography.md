# 🔐 Cryptography Challenges

> **Kategoriya:** Cryptography  
> **Challenges:** interencdec · Mod 26 · The Numbers · 13  
> **Daraja:** Beginner → Intermediate  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## Umumiy tushuncha

Cryptography — ma'lumotni shifrlash va deshifrlash ilmi. CTF'larda ko'pincha klassik sifar turlari (Caesar, ROT13, Base64, Vigenere) ishlatiladi. Zamonaviy kriptografiyadan farqli ravishda, bu challengelar asosiy encoding va klassik shifrlarni o'rganish uchun mo'ljallangan.

---

## 1. interencdec

### 📋 Challenge tavsifi
Nom o'zidan ko'rinib turibdi: **inter**net **enc**oding/**dec**oding. Flag turli encoding qatlamlari bilan yashirilgan — har birini ketma-ket decode qilish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Berilgan matnni ko'ramiz:**
```
YidkfnMUYSwdmRVZmRVZUYSwdmRVZmRVZUYSwdmRVZm...
```

**2-qadam — Encoding turini aniqlaymiz:**
`=` bilan tugasa yoki faqat A-Z, a-z, 0-9, +, / belgilar bo'lsa → **Base64**

**3-qadam — Base64 decode:**
```bash
echo "YidkfnMUYSwdmRVZmRVZUYSwdmRVZmRVZ..." | base64 -d
# Natija: yana encoded matn chiqishi mumkin
```

**4-qadam — Rot13 decode:**
Ba'zan Base64 decode qilgandan keyin ROT13 encoded matn chiqadi:
```bash
echo "decoded_text" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
# yoki CyberChef → ROT13
```

**5-qadam — Natija:**
```
picoCTF{...}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| `base64 -d` | Base64 decode |
| `tr` | ROT13 decode |
| [CyberChef](https://gchq.github.io/CyberChef) | Magic → avtomatik aniqlash |

### 💡 Xulosa va o'rganilgan narsalar
> Ko'p qatlamli encoding — CTF'larda keng tarqalgan. CyberChef'ning **"Magic"** funksiyasi encoding turini avtomatik aniqlaydi va dekod qiladi. Real hayotda ham ma'lumotlar bir necha marta encode qilingan holda uzatilishi mumkin.

**Kalit tushunchalar:** Base64, ROT13, multi-layer encoding, CyberChef Magic

---

## 2. Mod 26

### 📋 Challenge tavsifi
**Mod 26** — bu ROT13 shifrining matematik ifodasi. Alifbodagi har bir harf 13 pozitsiyaga suriladi. 26 harfli alifboda 13+13=26 bo'lgani uchun, ROT13 ni ikki marta qo'llasangiz asl matnga qaytasiz.

### 🔧 ROT13 nima?
```
A → N    N → A
B → O    O → B
C → P    P → C
...
M → Z    Z → M

Misol:
"HELLO" → "URYYB"
"URYYB" → "HELLO"  (teskari ham xuddi shunday!)
```

### 🔍 Yechim jarayoni

**1-qadam — Encoded flagni olamiz:**
```
cvpbPGS{arkg_gvzr_v'yy_gel_2_ebhaqf_bs_ebg13_nSkgmDJE}
```

**2-qadam — ROT13 decode qilamiz:**

```bash
# Terminal:
echo "cvpbPGS{arkg_gvzr_v'yy_gel_2_ebhaqf_bs_ebg13_nSkgmDJE}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# Python:
import codecs
print(codecs.decode("cvpbPGS{...}", 'rot_13'))

# Online: rot13.com
```

**3-qadam — Natija:**
```
picoCTF{next_time_i'll_try_2_rounds_of_rot13_aFxtzWQR}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Buyruq/URL |
|------|-----------|
| `tr` | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| Python `codecs` | `codecs.decode(text, 'rot_13')` |
| [rot13.com](https://rot13.com) | Online decoder |
| CyberChef | ROT13 operatsiyasi |

### 💡 Xulosa va o'rganilgan narsalar
> ROT13 — xavfsizlik uchun emas, faqat matnni ko'zdan yashirish uchun ishlatiladi (spoilerlarni yashirish kabi). Matematik asosi: `f(x) = (x + 13) mod 26`. Ikki marta qo'llash asl matnga qaytaradi: `ROT13(ROT13(x)) = x`.

**Kalit tushunchalar:** ROT13, Caesar cipher, modular arithmetic, `tr` buyrug'i

---

## 3. The Numbers

### 📋 Challenge tavsifi
Rasm yoki fayl berilgan — ichida raqamlar ketma-ketligi bor. Har bir raqam alifbodagi harfga mos keladi: `1=A, 2=B, 3=C ... 26=Z`. Bu **A1Z26** shifri deyiladi.

### 🔧 A1Z26 shifri:
```
A=1   B=2   C=3   D=4   E=5   F=6   G=7
H=8   I=9   J=10  K=11  L=12  M=13  N=14
O=15  P=16  Q=17  R=18  S=19  T=20  U=21
V=22  W=23  X=24  Y=25  Z=26
```

### 🔍 Yechim jarayoni

**1-qadam — Raqamlarni ko'ramiz:**
```
16 9 3 15 3 20 6 { 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14 }
```

**2-qadam — Har bir raqamni harfga o'giramiz:**
```
16=P  9=I  3=C  15=O  3=C  20=T  6=F
20=T  8=H  5=E  14=N  21=U  13=M  2=B  5=E  18=R  19=S  13=M  1=A  19=S  15=O  14=N
```

**3-qadam — Python bilan avtomatlashtirish:**
```python
numbers = [16, 9, 3, 15, 3, 20, 6, 20, 8, 5, 14, 21, 13, 2, 5, 18, 19, 13, 1, 19, 15, 14]
flag = ''.join(chr(n + 64) for n in numbers)
print(flag)
# PICOCTF{THENUMBERSMASON}
```

**4-qadam — Natija:**
```
PICOCTF{THENUMBERSMASON}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Maqsad |
|------|--------|
| Python | `chr(n + 64)` — raqamdan harfga |
| CyberChef | A1Z26 Cipher Decode |
| [dcode.fr](https://www.dcode.fr/letter-number-cipher) | Online A1Z26 decoder |

### 💡 Xulosa va o'rganilgan narsalar
> A1Z26 — eng oddiy substitution shifr. `chr()` funksiyasi Python da ASCII koddan belgiga o'giradi: `chr(65)='A'`, shuning uchun `chr(n + 64)` n=1 uchun 'A' beradi. **"The Numbers, Mason!"** — bu Call of Duty: Black Ops filmidan kelgan ibora!

**Kalit tushunchalar:** A1Z26 cipher, substitution cipher, Python `chr()`, ASCII

---

## 4. 13

### 📋 Challenge tavsifi
Nom o'zidayoq javob — **13** → ROT13! Mod 26 challengiga o'xshash, lekin yanada to'g'ridan-to'g'ri ROT13 decode qilish kerak.

### 🔍 Yechim jarayoni

**1-qadam — Encoded matnni olamiz:**
```
cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}
```

**2-qadam — ROT13 decode:**
```bash
# Terminal (eng tez):
echo "cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# Python:
import codecs
print(codecs.decode("cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}", 'rot_13'))
```

**3-qadam — Natija:**
```
picoCTF{not_too_bad_of_a_problem}
```
✅ Flag topildi!

### 🛠️ Ishlatilgan toollar
| Tool | Buyruq |
|------|--------|
| `tr` | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| Python | `codecs.decode(text, 'rot_13')` |

### 💡 Xulosa va o'rganilgan narsalar
> ROT13 — `picoCTF` → `cvpbPGS` bo'ladi. Shuning uchun flag boshida `cvpbPGS{` ko'rsangiz — darhol ROT13 decode qiling! Bu CTF'lardagi eng klassik encoding.

**Tez aniqlash:** `cvpbPGS{` → har doim ROT13! ⭐

**Kalit tushunchalar:** ROT13, Caesar cipher variant, quick recognition

---

## 📊 Umumiy xulosa — Cryptography

| Challenge | Shifr turi | Decode usuli |
|-----------|-----------|--------------|
| interencdec | Base64 + ROT13 | `base64 -d` → `tr` |
| Mod 26 | ROT13 | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| The Numbers | A1Z26 | `chr(n + 64)` |
| 13 | ROT13 | `codecs.decode(text, 'rot_13')` |

### Tez aniqlash guide
```
cvpbPGS{      → ROT13 ✅
YWJj...==     → Base64 ✅
16 9 3 15...  → A1Z26 ✅
%XX           → URL encoding ✅
&#XX;         → HTML entities ✅
```

### Foydali resurslar
- [CyberChef Magic](https://gchq.github.io/CyberChef/#recipe=Magic()) — avtomatik aniqlash ⭐
- [dcode.fr](https://www.dcode.fr) — 500+ shifr decoder
- [cryptii.com](https://cryptii.com) — visual decoder

---

*PicoCTF | Cryptography Challenges*
