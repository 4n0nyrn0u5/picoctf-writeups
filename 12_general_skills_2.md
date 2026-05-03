# ⚙️ General Skills — Batch 2

> **Kategoriya:** General Skills  
> **Challenges:** what's a net cat? · Wave a flag · First Grep · Tab Tab Attack · runme.py · convertme.py · fixme1.py · fixme2.py · Magikarp Ground Mission · Collaborative Development · Blame Game · First Find · Big Zip  
> **Daraja:** Beginner  
> **Muallif:** [4n0nyrn0u5](https://github.com/4n0nyrn0u5)

---

## 1. what's a net cat?

### 📋 Challenge tavsifi
Netcat nima ekanligini tushuntiruvchi challenge. Server bilan netcat orqali ulanib flag olinadi.

### 🔍 Yechim jarayoni
```bash
nc jupiter.challenges.picoctf.org PORT
# Flag avtomatik chiqadi:
# picoCTF{nEtCat_iS_a_tOol_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Netcat (nc) — tarmoq Swiss Army Knife. TCP/UDP ulanish, port tekshirish, fayl uzatish uchun ishlatiladi. CTF'da serverga ulanishning eng oddiy usuli.

**Kalit tushunchalar:** Netcat, TCP connection, nc

---

## 2. Wave a flag

### 📋 Challenge tavsifi
Binary fayl beriladi. `--help` yoki `-h` flagi bilan ishga tushirilganda flag chiqadi.

### 🔍 Yechim jarayoni
```bash
chmod +x warm
./warm -h
# Hello user! Pass me a -h to learn what I can do!
./warm --help
# picoCTF{b1scu1ts_4nd_gr4vy_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Linux dasturlarida `-h` yoki `--help` — yordam ma'lumotlarini ko'rsatadi. `chmod +x` — bajarish ruxsatini beradi.

**Kalit tushunchalar:** chmod, --help flag, Linux binary

---

## 3. First Grep

### 📋 Challenge tavsifi
Katta matn faylida flag yashirilgan. `grep` buyrug'i bilan qidirish kerak.

### 🔍 Yechim jarayoni
```bash
grep "picoCTF{" file.txt
# picoCTF{grep_is_good_to_find_things_...}
```
✅ Flag topildi!

### 💡 Xulosa
> `grep` — matn qidirish buyrug'i. Katta fayllardan pattern topishda eng tez usul. `-r` flagi bilan rekursiv qidirish ham mumkin.

```bash
grep -r "picoCTF{" ./          # Papkada rekursiv
grep -i "flag" file.txt        # Case-insensitive
grep -n "picoCTF" file.txt     # Qator raqami bilan
```

**Kalit tushunchalar:** grep, pattern matching, text search

---

## 4. Tab, Tab, Attack

### 📋 Challenge tavsifi
Juda uzun fayl nomi bor. Tab tugmasi bilan avtomatik to'ldirish (autocomplete) ishlatib faylni topish kerak.

### 🔍 Yechim jarayoni
```bash
ls
# Fjr.../ (uzun nomdagi papka)

cd F[TAB]    # Tab bosib avtomatik to'ldirish
cd F[TAB][TAB]  # Ichki papkaga ham Tab
ls
./fang-of-pickles[TAB]    # Faylni Tab bilan topamiz
# picoCTF{l3v3l_up_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Tab autocomplete — Linux terminalining eng muhim shortcut'laridan biri. Uzoq fayl nomlarini yozmasdan topish imkonini beradi.

**Kalit tushunchalar:** Tab autocomplete, Linux navigation, long filenames

---

## 5. runme.py

### 📋 Challenge tavsifi
Python fayl beriladi. Uni ishga tushirish kifoya.

### 🔍 Yechim jarayoni
```bash
python3 runme.py
# picoCTF{run_as_fast_as_you_can_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Python skriptlarini ishga tushirish: `python3 script.py`. Ba'zan `python` (v2) yoki `python3` (v3) ishlatiladi.

**Kalit tushunchalar:** Python execution, python3

---

## 6. convertme.py

### 📋 Challenge tavsifi
Python skript beriladi — raqamni bir sanoq tizimidan boshqasiga o'girish kerak (binary, decimal, hex).

### 🔍 Yechim jarayoni
```bash
python3 convertme.py
# Enter the integer representation of 0b110 in decimal: 6
# That is correct! Here's your flag:
# picoCTF{4ll_y0ur_b4s3_...}
```

**Konversiya:**
```python
int('0b110', 2)   # Binary → Decimal: 6
int('0x1f', 16)   # Hex → Decimal: 31
bin(6)            # Decimal → Binary: '0b110'
hex(31)           # Decimal → Hex: '0x1f'
```
✅ Flag topildi!

### 💡 Xulosa
> Python'da sanoq tizimi konversiyalari: `int(x, base)` — istalgan bazadan o'nlikka. `bin()`, `oct()`, `hex()` — o'nlikdan boshqasiga.

**Kalit tushunchalar:** Number base conversion, binary, decimal, hex, Python

---

## 7. fixme1.py

### 📋 Challenge tavsifi
Python skriptda sintaks xatosi bor. Xatoni topib tuzatish kerak.

### 🔍 Yechim jarayoni
```bash
python3 fixme1.py
# IndentationError: expected an indented block
```

Faylni ochamiz:
```python
# Xato:
if flag == "":
print("String is empty")  # ← Indentation yo'q!

# To'g'ri:
if flag == "":
    print("String is empty")  # ← 4 bo'sh joy
```

```bash
python3 fixme1.py
# picoCTF{1nd3nt1ty_cr1s1s_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Python'da indentation (bo'sh joy) — sintaksning muhim qismi. 4 ta bo'sh joy yoki 1 tab ishlatiladi. `IndentationError` — eng keng tarqalgan Python xatosi.

**Kalit tushunchalar:** Python indentation, syntax error, debugging

---

## 8. fixme2.py

### 📋 Challenge tavsifi
fixme1 dan keyingi daraja — boshqa turdagi sintaks xatosi.

### 🔍 Yechim jarayoni
```bash
python3 fixme2.py
# SyntaxError: invalid syntax
```

Faylni ochamiz:
```python
# Xato:
if flag = "picoCTF":    # ← = o'rniga == bo'lishi kerak!

# To'g'ri:
if flag == "picoCTF":   # ← Taqqoslash uchun ==
```

```bash
python3 fixme2.py
# picoCTF{3qu4l1ty_4qu4l1ty_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Python'da `=` — qiymat berish, `==` — taqqoslash. Bu klassik xato.

```python
x = 5      # Assignment (qiymat berish)
x == 5     # Comparison (taqqoslash) → True/False
```

**Kalit tushunchalar:** Python syntax, assignment vs comparison, debugging

---

## 9. Magikarp Ground Mission

### 📋 Challenge tavsifi
SSH orqali ulanib, turli papkalar bo'ylab flag bo'laklarini yig'ish kerak.

### 🔍 Yechim jarayoni
```bash
ssh ctf-player@venus.picoctf.net -p PORT

# 1-bo'lak:
cat 1of3.flag.txt
# picoCTF{xxsh_
ls somewhere/
cd somewhere/
cat 2of3.flag.txt
# 0r_4n0th3r_
# Hint: /home/ctf-player/drop-in/
cat /home/ctf-player/drop-in/3of3.flag.txt
# xxsh_e4a5a}
```

**Birlashtirish:**
```
picoCTF{xxsh_0r_4n0th3r_xxsh_e4a5a}
```
✅ Flag topildi!

### 💡 Xulosa
> Ko'p bo'lakli flag — tizimli qidirish muhim. `find` buyrug'i bilan barcha `.flag.txt` fayllarni tezda topish mumkin:
```bash
find / -name "*.flag.txt" 2>/dev/null
```

**Kalit tushunchalar:** SSH, multi-part flag, file navigation, find

---

## 10. Collaborative Development

### 📋 Challenge tavsifi
Git repository beriladi. Turli branch'larda flag bo'laklari yashirilgan.

### 🔍 Yechim jarayoni
```bash
git branch -a
# feature/part-1
# feature/part-2
# feature/part-3

git checkout feature/part-1
cat flag.py   # Part 1: picoCTF{t3@mw0rk_
git checkout feature/part-2
cat flag.py   # Part 2: m@k3s_th3_dr3@m_
git checkout feature/part-3
cat flag.py   # Part 3: w0rk_...}
```

**Birlashtirish:**
```
picoCTF{t3@mw0rk_m@k3s_th3_dr3@m_w0rk_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Git branch'lari — parallel development uchun. `git branch -a` barcha branch'larni ko'rsatadi. Real hayotda ham turli branch'larda maxfiy ma'lumotlar qolishi mumkin.

```bash
git branch -a          # Barcha branch'lar
git checkout branchname # Branch'ga o'tish
git log --all --oneline # Barcha commitlar
```

**Kalit tushunchalar:** Git branches, git checkout, collaborative git

---

## 11. Blame Game

### 📋 Challenge tavsifi
Git repository beriladi. `git blame` yordamida kim, qachon, qaysi qatorni o'zgartirganini topish kerak — flagni kim qo'shganini aniqlash.

### 🔍 Yechim jarayoni
```bash
git log --oneline
git blame flag.py
# abc1234 (picoCTF{...} 2024-01-01 12:00:00 +0000 1) # flag here

# Yoki:
git log -p | grep picoCTF
# picoCTF{@sk_th3_1nt3rn_...}
```
✅ Flag topildi!

### 💡 Xulosa
> `git blame` — har bir qatorni kim yozganini ko'rsatadi. Incident response va code audit da juda foydali.

```bash
git blame file.py           # Qator bo'yicha muallif
git log -p                  # Commit + diff
git log --all -p | grep flag # Barcha commitlarda qidirish
```

**Kalit tushunchalar:** git blame, git log -p, code audit

---

## 12. First Find

### 📋 Challenge tavsifi
Ko'p papkali tizimda bitta fayl yashirilgan. `find` buyrug'i bilan topish kerak.

### 🔍 Yechim jarayoni
```bash
find . -name "*.txt" 2>/dev/null
# ./some/deep/path/uber-secret.txt

cat ./some/deep/path/uber-secret.txt
# picoCTF{f1nd_15_f4st_...}
```
✅ Flag topildi!

### 💡 Xulosa
> `find` — fayl qidiruvning eng kuchli buyrug'i. CTF va pentestda zarur.

```bash
find . -name "flag.txt"         # Nom bo'yicha
find . -type f -name "*.txt"    # Faqat fayllar
find . -size +1M                # O'lcham bo'yicha
find . -newer reference.txt     # Yangi fayllar
find / -name "flag*" 2>/dev/null # Root dan qidirish
```

**Kalit tushunchalar:** find command, file search, directory traversal

---

## 13. Big Zip

### 📋 Challenge tavsifi
Katta ZIP arxivda flag yashirilgan. Arxivni ochib, ichidagi ko'p fayllar orasidan flagni topish kerak.

### 🔍 Yechim jarayoni
```bash
unzip big-zip-files.zip
# Ko'p fayl chiqadi...

# grep bilan barcha faylda qidirish:
grep -r "picoCTF{" .
# ./folder/s/e/c/r/e/t/with_the_flag.txt:picoCTF{gr3p_15_m4g1c_...}
```
✅ Flag topildi!

### 💡 Xulosa
> Katta arxivlar uchun `unzip` + `grep -r` kombinatsiyasi — eng tez yechim. Minglab faylni birma-bir ochishga hojat yo'q.

```bash
unzip archive.zip              # Arxivni ochish
unzip -l archive.zip           # Tarkibini ko'rish (ochmay)
grep -r "picoCTF{" .           # Rekursiv qidirish
grep -rl "picoCTF" .           # Faqat fayl nomlarini
```

**Kalit tushunchalar:** unzip, grep -r, recursive search, zip archives

---

## 📊 Umumiy xulosa

| Challenge | Texnika | Asosiy buyruq |
|-----------|---------|---------------|
| what's a net cat? | Netcat | `nc server port` |
| Wave a flag | Help flag | `./binary --help` |
| First Grep | Pattern search | `grep "picoCTF{" file` |
| Tab Tab Attack | Autocomplete | `Tab` tugmasi |
| runme.py | Python run | `python3 script.py` |
| convertme.py | Base conversion | `int('0b110', 2)` |
| fixme1.py | Indentation fix | 4 bo'sh joy |
| fixme2.py | Syntax fix | `==` vs `=` |
| Magikarp Ground Mission | SSH + navigation | `find / -name "*.txt"` |
| Collaborative Development | Git branches | `git branch -a` |
| Blame Game | Git history | `git blame`, `git log -p` |
| First Find | File search | `find . -name "*.txt"` |
| Big Zip | Archive + grep | `unzip` + `grep -r` |

---

*PicoCTF | General Skills | Batch 2*
