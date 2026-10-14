\# Лаборатори №6: Тест тохиолдол боловсруулах — Category-Partition ба PICT



\## Оюутны мэдээлэл



\- Нэр: ANAR BATBOLD

\- Код: B170910823

\- Хичээл: F.CSA313 Программ хангамжийн чанарын баталгаа ба тест



\---



\# Хэсэг A: Category-Partition — Шилжүүлгийн функц



\## Алдааны давуу эрэмбэ



Нэг хүсэлтэд хэд хэдэн алдаа зэрэг орсон тохиолдолд дараах эрэмбээр алдаа буцна гэж үзэв.



```text

ERROR\_NO\_FROM

→ ERROR\_FROZEN

→ ERROR\_NO\_TO

→ ERROR\_SAME\_ACCOUNT

→ ERROR\_BAD\_AMOUNT

→ ERROR\_INSUFFICIENT

→ ERROR\_LIMIT

```



\### Үндэслэл



Эх данс байхгүй эсвэл царцаагдсан үед бусад нөхцөлийг шалгах боломжгүй тул хамгийн түрүүнд `from` данстай холбоотой алдаанууд шалгагдана. Дараа нь `to` дансны нөхцөл, хамгийн сүүлд мөнгөн дүнтэй холбоотой нөхцөлүүд шалгагдана.



\---



\## Сонголт ба төлөөлөх утгууд



| Параметр | Сонголт | Төлөөлөх утга |

|----------|----------|----------|

| From | Valid | Хүчинтэй данс |

| From | Frozen | Царцаагдсан данс |

| From | Missing | Байхгүй данс |

| To | Valid | Хүчинтэй данс |

| To | Missing | Байхгүй данс |

| To | SameAsFrom | Ижил данс |

| Amount | Normal | Хэвийн дүн |

| Amount | Zero | 0 эсвэл сөрөг дүн |

| Amount | OverBalance | Үлдэгдлээс их |

| Amount | OverLimit | Өдрийн лимитээс их |

| Currency | Same | Ижил валют |

| Currency | Different | Өөр валют |



\---



\## Хязгаарлалтууд



| Утга | Тэмдэглэгээ | Тайлбар |

|----------|----------|----------|

| Frozen | ERROR | Царцаагдсан данснаас шилжүүлэг хийх боломжгүй |

| Missing (From) | ERROR | Эх данс байхгүй |

| Missing (To) | ERROR | Хүлээн авагч данс байхгүй |

| SameAsFrom | ERROR | Өөрийн данс руу шилжүүлэх боломжгүй |

| Zero | ERROR | Amount > 0 байх ёстой |

| OverBalance | ERROR | Үлдэгдэл хүрэлцэхгүй |

| OverLimit | ERROR | Өдрийн лимит давсан |

| Currency=Different | SINGLE | Валют хөрвүүлэлттэй тусгай нөхцөл |

| From=Missing | IF | Amount утга ач холбогдолгүй болно |

| To=Missing | IF | Amount утга ач холбогдолгүй болно |



\---



\## Бүрэн олонлог ба хязгаарлалтын дараах тоо



Бүрэн олонлог:



```text

From      = 3

To        = 3

Amount    = 4

Currency  = 2



Нийт = 3 × 3 × 4 × 2 = 72

```



Хязгаарлалтын дараа зарим логикийн хувьд боломжгүй хослолууд хасагдана.



```text

72 → 60

```



Ингэснээр тестлэх шаардлагатай орчны тоо буурна.



\---



\## Спецификацийн хүснэгт



| # | From | To | Amount | Currency | Хүлээгдэх үр дүн |

|---|---|---|---|---|---|

| 1 | Valid | Valid | Normal | Same | 201 OK |

| 2 | Valid | Valid | Normal | Different | 201 OK |

| 3 | Frozen | Valid | Normal | Same | ERROR\_FROZEN |

| 4 | Missing | Valid | Normal | Same | ERROR\_NO\_FROM |

| 5 | Valid | Missing | Normal | Same | ERROR\_NO\_TO |

| 6 | Valid | SameAsFrom | Normal | Same | ERROR\_SAME\_ACCOUNT |

| 7 | Valid | Valid | Zero | Same | ERROR\_BAD\_AMOUNT |

| 8 | Valid | Valid | OverBalance | Same | ERROR\_INSUFFICIENT |

| 9 | Valid | Valid | OverLimit | Same | ERROR\_LIMIT |

|10 | Frozen | SameAsFrom | OverLimit | Different | ERROR\_FROZEN |

|11 | Missing | Missing | Normal | Different | ERROR\_NO\_FROM |

|12 | Valid | Missing | OverBalance | Same | ERROR\_NO\_TO |



\---



\# Хэсэг B: PICT



\## Browser Model



```text

Content:   Allow, Restrict, Block

Cookies:   Allow, Restrict, Block

Popups:    Yes, No

Addons:    Yes, No

Attacks:   Yes, No

Forgeries: Yes, No

```



\---



\## Browser Model Results



| Хувилбар | Мөрийн тоо |

|----------|-----------|

| 2-way | 10 |

| 3-way | 20 |

| Full Combination | 144 |



\### Greedy алгоритмын тайлбар



Бүрэн олонлогийн хэмжээ:



```text

3 × 3 × 2 × 2 × 2 × 2 = 144

```



PICT нь pairwise хамралтыг хангахын тулд ердөө 10 мөр үүсгэсэн. 3-way үед 20 мөр болсон. Энэ нь бүрэн олонлогоос мэдэгдэхүйц бага тоо юм. Гэсэн хэдий ч PICT нь greedy алгоритм ашигладаг тул хамгийн бага мөрийн тоо өгнө гэсэн баталгаа байхгүй. Тиймээс гараар боловсруулсан хүснэгтээс олон мөр өгөх боломжтой.



\---



\## Transfer Models



\### Мөрийн тооны харьцуулалт



| Модель | Мөрийн тоо |

|----------|-----------|

| transfer-nc.model | 13 |

| transfer.model | 13 |

| transfer-neg.model | 16 |



\### Ажиглалт



`transfer.model` нь хязгаарлалттай боловч pairwise generation үед мөрийн тоо өөрчлөгдөөгүй.



`transfer-neg.model` дээр `\~` тэмдэг ашигласнаар нэг мөрөнд олон ERROR утга давхардах боломж буурсан. Ингэснээр error masking багасч, category-partition дахь ERROR утгуудтай ойролцоо тестүүд үүссэн.



\---



\# PICT гаралтаас бодит тест кейсүүд



\## Test Case 1 — Happy Path



\### Setup



\- Эх данс хүчинтэй

\- Хүлээн авагч хүчинтэй

\- Үлдэгдэл хүрэлцээтэй

\- Өдрийн лимит хүрэлцээтэй



\### Input



```text

From=Valid

To=Valid

Amount=Normal

Currency=Same

```



\### Oracle



```http

201

```



```json

{

&#x20; "result": "OK"

}

```



\---



\## Test Case 2 — Эх данс байхгүй



\### Setup



Эх данс бүртгэлгүй.



\### Input



```text

From=Missing

To=Valid

Amount=Normal

Currency=Same

```



\### Oracle



```json

{

&#x20; "result": "ERROR\_NO\_FROM"

}

```



\---



\## Test Case 3 — Ижил данс руу шилжүүлэх



\### Setup



Эх болон хүлээн авагч данс ижил.



\### Input



```text

From=Valid

To=SameAsFrom

Amount=Normal

Currency=Different

```



\### Oracle



```json

{

&#x20; "result": "ERROR\_SAME\_ACCOUNT"

}

```



\---



\# Category-Partition ба PICT-ийн харьцуулалт



| Category-Partition | PICT |

|----------|----------|

| Алдааны семантик сайн илэрхийлдэг | Pairwise хамралтыг баталгаажуулдаг |

| Business rule тодорхойлох боломжтой | Автоматаар тест үүсгэдэг |

| Гар ажиллагаа ихтэй | Хурдан covering array гаргадаг |

| Error oracle тодорхойлоход тохиромжтой | Хослолын хамралтад тохиромжтой |



\---



\# Дүгнэлт



Энэхүү лабораторийн ажлаар Category-Partition болон Pairwise Test Design гэсэн хоёр өөр аргыг практикт хэрэгжүүллээ. Category-Partition арга нь системийн оролтыг ангилан, төлөөлөх утгууд болон алдааны ангиллыг тодорхойлох боломж олгосон. Мөн хязгаарлалтуудыг тодорхойлсноор бодит бус тестийн хослолуудыг эрт шатанд хасаж чадсан. PICT хэрэгслийг ашигласнаар бүх боломжит хослолыг турших шаардлагагүйгээр pairwise хамралтыг автоматаар үүсгэж чадлаа. Browser моделийн хувьд бүрэн олонлог 144 байсан боловч PICT 2-way үед ердөө 10 мөр, 3-way үед 20 мөр үүсгэсэн. Transfer моделийн хувьд сөрөг утгуудыг `\~` тэмдэглэгээгээр тодорхойлсноор error masking буурсан. Category-Partition нь business rule болон алдааны семантикийг илүү сайн илэрхийлдэг болох нь харагдсан. Харин PICT нь хосын хамралтыг маш хурдан хангаж, тестийн тоог бууруулах давуу талтай байлаа. Хамгийн их бодол шаардсан хэсэг нь давуу эрэмбэ болон хязгаарлалтыг зөв тодорхойлох ажил байсан. Энэхүү лабораторийн үр дүнд тест дизайн хийх хоёр өөр аргын давуу болон сул талыг харьцуулан ойлгож авлаа.



\---



\# Хавсаргасан файлууд



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

