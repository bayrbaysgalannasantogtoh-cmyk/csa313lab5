lab 5

Оюутны мэдээлэл Овог нэр: Н.Баярбаясгалан Оюутны код: B242270012

Орчны хувилбар
OS: Ubuntu 26.04.1 LTS
Kernel: 6.18.33.2-microsoft-standard-WSL2
Node.js: v22.22.1
NPM: 9.2.0
Newman: 6.2.2
Git: git version 2.53.0
Postman for Windows: Version 12.31.3


Тестийн төлөвлөгөө
1. Happy path: Идэвхтэй оюутан, нөхцөл бүрэн хангасан хичээлд бүртгүүлэх $\rightarrow$ Статус 201, result: "OK".   
2. Оюутан байхгүй: Бүртгэлгүй studentID өгөх $\rightarrow$ Статус 200, result: "ERROR_NO_STUDENT".   
3. Оюутан идэвхгүй: status: "inactive" оюутан $\rightarrow$ Статус 200, result: "ERROR_INACTIVE_STUDENT".   
4. Хичээл байхгүй: Баазад байхгүй courseID өгөх $\rightarrow$ Статус 200, result: "ERROR_NO_COURSE".   
5. Нөхцөл дутуу: Шаардлагатай хичээлийг үзээгүй оюутан $\rightarrow$ Статус 200, result: "ERROR_PREREQUISITES".   
6. Талбар дутуу: courseID эсвэл studentID талбаргүй хүсэлт илгээх $\rightarrow$ Статус 400, result: "ERROR_BAD_REQUEST".   
7. Буруу JSON: Формат нь эвдэрсэн JSON өгөгдөл илгээх $\rightarrow$ Статус 400, result: "ERROR_BAD_JSON".   
8. Нөхцөлгүй хичээл: Prerequisites нь хоосон [] байх үед амжилттай бүртгэх $\rightarrow$ Статус 201, result: "OK".   

Сонголт ба төлөөлөх утгын хүснэгт (Equivalence Partitioning & Boundary Values)

| Параметр / Нөхцөл | Төлөв (Partition) | Төлөөлөх утга | Төрөл |
|---|---|---|---|
| studentID | Системд бүртгэлтэй | "B231234567" | Хүчинтэй |
| studentID | Системд бүртгэлгүй | "NOT_EXIST" | Хүчингүй |
| status | Идэвхтэй төлөв | "active" | Хүчинтэй |
| status | Идэвхгүй төлөв | "inactive" | Хүчингүй |
| courseID | Бүртгэлтэй хичээл | "CS313" | Хүчинтэй |
| courseID | Байхгүй хичээл | "NO_COURSE" | Хүчингүй |
| prerequisites | Урьдач хангасан | ["CS201"] | Хүчинтэй |
| prerequisites | Урьдач дутуу | [] | Хүчингүй |

Спецификацийн хүснэгт (Test Design & Specification)

| № | Тест кейсийн зорилго | Endpoint / Төрөл | HTTP Статус | Хүлээгдэх үр дүн |
|---|---|---|---|---|
| 1 | Амжилттай бүртгэл | POST /registrations | 201 | "OK", registrationID тоо |
| 2 | Бүртгэлгүй оюутан | POST /registrations | 200 | "ERROR_NO_STUDENT" |
| 3 | Идэвхгүй оюутан | POST /registrations | 200 | "ERROR_INACTIVE_STUDENT" |
| 4 | Байхгүй хичээл | POST /registrations | 200 | "ERROR_NO_COURSE" |
| 5 | Урьдач дутуу оюутан | POST /registrations | 200 | "ERROR_PREREQUISITES" |
| 6 | Талбар дутуу илгээх | POST /registrations | 400 | "ERROR_BAD_REQUEST" |
| 7 | Эвдэрсэн JSON илгээх | POST /registrations | 400 | "ERROR_BAD_JSON" |
| 8 | Урьдачгүй хичээл (Boundary) | POST /registrations | 201 | "OK" |
| 9 | Давхар алдааны шалгалт (Оюутан болон хичээл хоёулаа системд байхгүй) | `POST /registrations` (`studentID: "NO_STUDENT"`, `courseID: "NO_COURSE"`) | 200 OK | `result: "ERROR_NO_STUDENT"` (Сервер эхлээд оюутныг шалгаж алдааг буцаана) |

Newman үр дүнгийн хүснэгт

| Гаралтын файл | Туршилтын төрөл | Нийт хүсэлт | Assertions (Нийт/Fail) | Exit Code |
|---|---|---|---|---|
| results/newman-pass.txt | Хэвийн горим | 15 | 19 / 0 failed | 0 |
| results/newman-fail.txt | Санаатай алдаа | 15 | 19 / 1+ failed | 1 |
| results/newman-down.txt | Сервер унтарсан | - | - | 1 |

1. Сервер асаах
node server.js
2. Тестийг Newman-ээр ажиллуулах
newman run lab05-collection.json
newman run lab05-collection-fail.json
newman run lab05-collection.json 2>&1 | tee results/newman-pass.txt
newman run lab05-collection-fail.json 2>&1 | tee results/newman-fail.txt

Дүгнэлт

Энэ лабораторийн ажлаар Postman болон Newman ашиглан API тестүүд бичиж, терминалаас автоматаар шалгаж сурлаа. Тест зохиох явцад ангилал болон заагийн утгуудыг тооцоолох нь хамгийн их бодол шаардсан. Оюутны төлөв, үзсэн хичээлүүд, хичээлийн урьдач шаардлага зэрэг олон нөхцөлийг уялдуулж хүсэлт бүрийг тусад нь бэлдэх хэрэгтэй болсон. Бэлтгэх үед баазад бүртгэлгүй мөртлөө идэвхтэй байх оюутан эсвэл байхгүй хичээлийг урьдач болгох гэх мэт боломжгүй хослолууд таарч байсныг ялгаж авч үзсэн. Мөн Postman дээр baseUrl хувьсагчийн анхны утгыг дутуу бичсэнээс болж Newman дээр Invalid URI алдаа зааж байсныг олж зассан. Харин серверийн логикт ямар нэгэн согог, алдаа гараагүй. Урьдчилсан нөхцөл дутуу байх, буруу JSON илгээх зэрэг бүх тохиолдолд сервер төлөвлөсөн зөв хариуг буцааж байсан. Энэ ажлыг хийснээр тестүүдээ бүрэн автоматжуулж, терминалаас хэрхэн найдвартай ажиллуулж үр дүнгээ авахыг практик дээр сайн ойлголоо.
