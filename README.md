\# Лаборатори №6: Тест тохиолдол боловсруулах — Category-Partition ба PICT



\## Оюутны мэдээлэл



\- Нэр: ANAR BATBOLD

\- Код: B170910823

\- Хичээл: F.CSA313 Программ хангамжийн чанарын баталгаа ба тест



\---



\# Хэсэг A: Category-Partition — Шилжүүлгийн функц



\## Давуу эрэмбэ



Нэг хүсэлтэд олон алдаа зэрэг орсон тохиолдолд дараах эрэмбээр алдаа буцна гэж үзэв.



```text

from → to → amount

```



Үндэслэл: эхлээд эх данс хүчинтэй эсэхийг шалгахгүйгээр бусад нөхцөлийг үнэлэх боломжгүй.



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

|--------|--------|--------|

| Frozen | ERROR | Шилжүүлэг хийх боломжгүй |

| Missing (From) | ERROR | Эх данс байхгүй |

| Missing (To) | ERROR | Хүлээн авагч байхгүй |

| SameAsFrom | ERROR | Өөртөө шилжүүлэх боломжгүй |

| Zero | ERROR | Amount > 0 байх ёстой |

| OverBalance | ERROR | Үлдэгдэл хүрэлцэхгүй |

| OverLimit | ERROR | Өдрийн лимит давсан |

| Currency=Different | SINGLE | Хөрвүүлэлт хийх тохиолдол |

| From=Missing | IF | Amount нөхцөл ач холбогдолгүй |

| To=Missing | IF | Amount нөхцөл ач холбогдолгүй |



\---



\## Бүрэн олонлог ба хязгаарлалтын дараах тоо



Параметрийн бүх боломжит хослол:



```text

3 × 3 × 4 × 2 = 72

```



Хязгаарлалтууд хэрэглэсний дараа:



```text

72 → 60

```



Тестлэх шаардлагатай тохиолдлын тоо буурсан.



\---



\## Спецификацийн хүснэгт



| # | From | To | Amount | Currency | Хүлээгдэх үр дүн |

|---|---|---|---|---|---|

| 1 | Valid | Valid | Normal | Same | 201 OK |

| 2 | Frozen | Valid | Normal | Same | ERROR\_FROZEN |

| 3 | Missing | Valid | Normal | Same | ERROR\_NO\_FROM |

| 4 | Valid | Missing | Normal | Same | ERROR\_NO\_TO |

| 5 | Valid | SameAsFrom | Normal | Same | ERROR\_SAME\_ACCOUNT |

| 6 | Valid | Valid | Zero | Same | ERROR\_BAD\_AMOUNT |

| 7 | Valid | Valid | OverBalance | Same | ERROR\_INSUFFICIENT |

| 8 | Valid | Valid | OverLimit | Same | ERROR\_LIMIT |

| 9 | Valid | Valid | Normal | Different | 201 OK |

| 10 | Frozen | SameAsFrom | OverLimit | Different | ERROR\_FROZEN |

| 11 | Missing | Missing | Normal | Different | ERROR\_NO\_FROM |

| 12 | Valid | Missing | OverLimit | Same | ERROR\_NO\_TO |



\---



\# Хэсэг B: PICT



\## Browser Model



```text

Content: Allow, Restrict, Block

Cookies: Allow, Restrict, Block

Popups: Yes, No

Addons: Yes, No

Attacks: Yes, No

Forgeries: Yes, No

```



\---



\## Browser Model Results



| Model | Мөрийн тоо |

|---------|---------|

| 2-way | 10 |

| 3-way | 20 |

| Full Combination | 144 |



\### Greedy тайлбар



Бүрэн олонлог нь:



```text

3 × 3 × 2 × 2 × 2 × 2 = 144

```



болно.



PICT нь 2-way covering array-г ердөө 10 мөр, 3-way covering array-г 20 мөрөөр үүсгэсэн. Энэ нь бүрэн олонлогоос мэдэгдэхүйц бага боловч PICT нь greedy алгоритм ашигладаг тул хамгийн бага мөрийн тоо өгнө гэсэн баталгаа байхгүй. Иймээс лекц дээр гараар боловсруулсан хувилбараас илүү мөр үүсгэх боломжтой.



\---



\## Transfer Models



\### transfer-nc.model



| Model | Мөрийн тоо |

|---------|---------|

| transfer-nc | 13 |



\### transfer.model



| Model | Мөрийн тоо |

|---------|---------|

| transfer | 13 |



\### transfer-neg.model



| Model | Мөрийн тоо |

|---------|---------|

| transfer-neg | 16 |



\---



\## Харьцуулалт



| Хувилбар | Мөрийн тоо |

|----------|-----------|

| Browser 2-way | 10 |

| Browser 3-way | 20 |

| Transfer NC | 13 |

| Transfer Constraint | 13 |

| Transfer Negative | 16 |



`transfer-neg.model` дээр `\~` тэмдэг ашигласнаар нэг мөрөнд олон алдаа зэрэг оролцох магадлал багассан. Энэ нь error masking-ийг бууруулж, category-partition дахь ERROR утгуудтай дүйцэж байна.



\---



\# PICT гаралтаас бодит тест кейсүүд



\## Test Case 1 - Happy Path



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



```json

{

&#x20; "result": "OK"

}

```



HTTP:



```text

201

```



\---



\## Test Case 2 - Missing Source Account



\### Setup



Эх данс байхгүй.



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



\## Test Case 3 - Same Account



\### Setup



Ижил данс руу шилжүүлэх оролдлого.



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



\# Дүгнэлт



Энэхүү лабораторийн ажлаар Category-Partition болон Pairwise Test Design гэсэн хоёр өөр тест сонголтын аргыг практикт хэрэглэж үзлээ. Category-Partition арга нь спецификаци доторх нөхцөлүүдийг задлан ангилж, тестийн оролтуудыг системтэй тодорхойлох боломж олгосон. Тухайн аргаар алдааны ангилал, төлөөлөх утгууд болон хязгаарлалтуудыг тодорхойлсон. PICT ашигласнаар бүх боломжит хослолыг турших шаардлагагүйгээр pairwise хамралтыг хангах боломжтой болсон. Browser моделийн хувьд бүрэн олонлог 144 мөр байсан ч PICT 2-way үед 10 мөр, 3-way үед 20 мөр үүсгэсэн. Transfer моделийн хувьд хязгаарлалт болон сөрөг утгуудыг тусгаж олон хослолыг автоматаар үүсгэв. `\~` тэмдэглэгээ ашиглах нь олон алдаа нэг мөрөнд давхцах эрсдэлийг бууруулдаг болохыг ажигласан. Category-Partition нь алдааны семантик болон бизнес дүрмийг илүү сайн илэрхийлдэг. Харин PICT нь хосын хамралтыг автоматаар хангаж тестийн тоог бууруулах давуу талтай. Энэ лабораторийн хамгийн их бодол шаардсан хэсэг нь хязгаарлалт болон давуу эрэмбийг зөв тодорхойлох ажил байлаа.



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

