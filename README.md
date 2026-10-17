# Лаборатори №6: Тест тохиолдол боловсруулах — Category-Partition ба PICT

## Оюутны мэдээлэл

- Нэр: ANAR BATBOLD
- Код: B170910823
- Хичээл: F.CSA313 Программ хангамжийн чанарын баталгаа ба тест

---

# Хэсэг A: Category-Partition — Шилжүүлгийн функц

## Алдааны давуу эрэмбэ

Нэг хүсэлтэд хэд хэдэн алдаа зэрэг орсон тохиолдолд дараах эрэмбээр алдаа буцна гэж үзэв.

```text
ERROR_NO_FROM
→ ERROR_FROZEN
→ ERROR_NO_TO
→ ERROR_SAME_ACCOUNT
→ ERROR_BAD_AMOUNT
→ ERROR_INSUFFICIENT
→ ERROR_LIMIT
```

### Үндэслэл

Эх данс байхгүй эсвэл царцаагдсан үед бусад нөхцөлийг шалгах боломжгүй тул хамгийн түрүүнд from данстай холбоотой нөхцөлүүдийг шалгана. Дараа нь хүлээн авагч данс, хамгийн сүүлд мөнгөн дүнтэй холбоотой нөхцөлүүдийг шалгана.

---

## Сонголт ба төлөөлөх утгууд

| Параметр | Сонголт | Төлөөлөх утга |
|----------|----------|----------|
| From | Valid | Хүчинтэй данс |
| From | Frozen | Царцаагдсан данс |
| From | Missing | Байхгүй данс |
| To | Valid | Хүчинтэй данс |
| To | Missing | Байхгүй данс |
| To | SameAsFrom | Ижил данс |
| Amount | Normal | Хэвийн дүн |
| Amount | Zero | 0 эсвэл түүнээс бага |
| Amount | OverBalance | Үлдэгдлээс их |
| Amount | OverLimit | Өдрийн лимитээс их |
| Currency | Same | Ижил валют |
| Currency | Different | Өөр валют |

---

## Хязгаарлалтууд

| Утга | Тэмдэглэгээ | Тайлбар |
|----------|----------|----------|
| Frozen | ERROR | Царцаагдсан данс |
| Missing (From) | ERROR | Эх данс байхгүй |
| Missing (To) | ERROR | Хүлээн авагч данс байхгүй |
| SameAsFrom | ERROR | Өөрийн данс руу шилжүүлэх |
| Zero | ERROR | Amount ≤ 0 |
| OverBalance | ERROR | Үлдэгдэл хүрэлцэхгүй |
| OverLimit | ERROR | Өдрийн лимит давсан |
| Currency=Different | SINGLE | Валют хөрвүүлэлт шаардагдана |
| From=Missing | IF | Amount утга ач холбогдолгүй |
| To=Missing | IF | Amount утга ач холбогдолгүй |

---

## Бүрэн олонлог ба хязгаарлалтын дараах тоо

Параметрүүдийн боломжит утгууд:

```text
From      = 3
To        = 3
Amount    = 4
Currency  = 2
```

Бүрэн олонлог:

```text
3 × 3 × 4 × 2 = 72
```

Хязгаарлалтууд:

```text
IF From = Missing THEN Amount = Normal
IF To = Missing THEN Amount = Normal
```

From эсвэл To нь Missing үед Amount-ийн бусад утгууд логикийн утгагүй болно.

Мөн:

```text
Currency = Different
```

утгыг SINGLE нөхцөл болгож үзсэн.

Иймээс бодит туршилтад шаардлагагүй болон утгагүй хослолууд хасагдсанаар:

```text
72 → ойролцоогоор 60
```

болно.

---

## Спецификацийн хүснэгт

| # | From | To | Amount | Currency | Хүлээгдэх үр дүн |
|---|---|---|---|---|---|
| 1 | Valid | Valid | Normal | Same | 201 OK |
| 2 | Valid | Valid | Normal | Different | 201 OK |
| 3 | Frozen | Valid | Normal | Same | ERROR_FROZEN |
| 4 | Missing | Valid | Normal | Same | ERROR_NO_FROM |
| 5 | Valid | Missing | Normal | Same | ERROR_NO_TO |
| 6 | Valid | SameAsFrom | Normal | Same | ERROR_SAME_ACCOUNT |
| 7 | Valid | Valid | Zero | Same | ERROR_BAD_AMOUNT |
| 8 | Valid | Valid | OverBalance | Same | ERROR_INSUFFICIENT |
| 9 | Valid | Valid | OverLimit | Same | ERROR_LIMIT |
|10 | Frozen | SameAsFrom | OverLimit | Different | ERROR_FROZEN |
|11 | Missing | Missing | Normal | Different | ERROR_NO_FROM |
|12 | Valid | Missing | OverBalance | Same | ERROR_NO_TO |
|13 | Valid | Valid | Normal | Different | 201 OK (Currency Conversion) |

---

# Хэсэг B: PICT

## Browser Model

```text
Content:   Allow, Restrict, Block
Cookies:   Allow, Restrict, Block
Popups:    Yes, No
Addons:    Yes, No
Attacks:   Yes, No
Forgeries: Yes, No
```

---

## Browser Model Results

| Хувилбар | Мөрийн тоо |
|----------|-----------|
| 2-way | 10 |
| 3-way | 20 |
| Full Combination | 144 |

### Greedy алгоритмын тайлбар

Бүрэн олонлог:

```text
3 × 3 × 2 × 2 × 2 × 2 = 144
```

PICT нь 2-way covering array-г 10 мөрөөр, 3-way covering array-г 20 мөрөөр үүсгэсэн. Энэ нь бүрэн олонлогоос хамаагүй бага байна. Гэсэн хэдий ч PICT нь greedy алгоритм ашигладаг тул хамгийн бага боломжит мөрийн тоог үргэлж гаргана гэсэн баталгаа байхгүй.

---

## Transfer Model Comparison

| Model | Rows | Happy Path Present | Max Errors Per Row | Constraints Needed |
|---------|---------|---------|---------|---------|
| transfer-nc | 13 | Yes | Multiple | Yes |
| transfer | 13 | Yes | Multiple | Yes |
| transfer-neg | 16 | Yes | One | Less Important |

### Analysis

transfer-nc болон transfer моделуудын зарим мөрөнд хэд хэдэн алдаа зэрэг орсон байна.

Жишээ:

```text
Missing + Missing + OverBalance
```

Энэ нь error masking үүсгэж болно.

transfer-neg моделд `~` тэмдэг ашигласнаар нэг мөрөнд хамгийн ихдээ нэг сөрөг утга орсон тул error masking буурсан. Тиймээс transfer-neg нь Category-Partition дахь ERROR утгуудтай илүү ойролцоо болсон.

---

# PICT гаралтаас бодит тест кейсүүд

## Test Case 1 – Happy Path

### PICT Row

```text
Valid | Valid | Normal | Same
```

### Setup

```text
Source account exists
Destination account exists
Balance = 100000 MNT
Daily limit remaining = 1000000 MNT
```

### Request

```json
{
  "from": "ACC001",
  "to": "ACC002",
  "amount": 10000,
  "currency": "MNT"
}
```

### Oracle

HTTP:

```text
201
```

Response:

```json
{
  "result": "OK"
}
```

---

## Test Case 2 – Missing Source Account

### PICT Row

```text
Missing | Valid | Normal | Same
```

### Setup

```text
Source account does not exist
Destination account exists
```

### Request

```json
{
  "from": "BAD001",
  "to": "ACC002",
  "amount": 10000,
  "currency": "MNT"
}
```

### Oracle

```json
{
  "result": "ERROR_NO_FROM"
}
```

---

## Test Case 3 – Same Account

### PICT Row

```text
Valid | SameAsFrom | Normal | Different
```

### Setup

```text
Source and destination accounts are identical
```

### Request

```json
{
  "from": "ACC001",
  "to": "ACC001",
  "amount": 10000,
  "currency": "USD"
}
```

### Oracle

```json
{
  "result": "ERROR_SAME_ACCOUNT"
}
```

---

# Category-Partition ба PICT-ийн харьцуулалт

| Category-Partition | PICT |
|----------|----------|
| Алдааны семантик сайн илэрхийлдэг | Pairwise хамралт баталгаажуулдаг |
| Business rule ойлгоход тохиромжтой | Автоматаар тест үүсгэдэг |
| Гар ажиллагаа ихтэй | Хурдан |
| Oracle тодорхойлоход сайн | Coverage сайтай |

---

# Дүгнэлт

Энэхүү лабораторийн ажлаар Category-Partition болон Pairwise Test Design гэсэн хоёр өөр аргыг практикт хэрэгжүүллээ. Category-Partition арга нь тестийн оролтуудыг ангилж, бизнес дүрмүүдийг системтэйгээр задлан шинжлэх боломж олгосон. Мөн [ERROR], [SINGLE], [IF] тэмдэглэгээг ашиглан боломжгүй болон утгагүй нөхцөлүүдийг ялгаж чадсан. PICT хэрэгсэл нь pairwise covering array автоматаар үүсгэж тестийн тоог мэдэгдэхүйц багасгасан. Browser моделийн хувьд бүрэн олонлог 144 мөр байсан боловч PICT 2-way үед 10 мөр, 3-way үед 20 мөр болгож бууруулсан. Transfer моделийн хувьд хязгаарлалтууд болон сөрөг утгуудыг ашигласнаар илүү утга бүхий тестүүд үүсэх боломжтой болсон. transfer-neg моделд `~` ашигласнаар error masking багассан. Category-Partition нь алдааны семантик болон бизнес дүрмийг илэрхийлэхэд илүү тохиромжтой байсан. Харин PICT нь хамралтыг баталгаажуулж тестийн тоог багасгах давуу талтай байв. Энэхүү лабораторийн явцад хамгийн их бодол шаардсан хэсэг нь хязгаарлалтууд болон давуу эрэмбийг зөв тодорхойлох ажил байлаа.

---

# Хавсаргасан файлууд

```text
browser.model
transfer-nc.model
transfer.model
transfer-neg.model

results/
├── pict-build.txt
├── browser-2way.txt
├── browser-3way.txt
├── transfer-nc.txt
├── transfer.txt
└── transfer-neg.txt
```