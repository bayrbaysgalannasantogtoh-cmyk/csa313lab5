lab 5

1. Happy path: Идэвхтэй оюутан, нөхцөл бүрэн хангасан хичээлд бүртгүүлэх $\rightarrow$ Статус 201, result: "OK".   
2. Оюутан байхгүй: Бүртгэлгүй studentID өгөх $\rightarrow$ Статус 200, result: "ERROR_NO_STUDENT".   
3. Оюутан идэвхгүй: status: "inactive" оюутан $\rightarrow$ Статус 200, result: "ERROR_INACTIVE_STUDENT".   
4. Хичээл байхгүй: Баазад байхгүй courseID өгөх $\rightarrow$ Статус 200, result: "ERROR_NO_COURSE".   
5. Нөхцөл дутуу: Шаардлагатай хичээлийг үзээгүй оюутан $\rightarrow$ Статус 200, result: "ERROR_PREREQUISITES".   
6. Талбар дутуу: courseID эсвэл studentID талбаргүй хүсэлт илгээх $\rightarrow$ Статус 400, result: "ERROR_BAD_REQUEST".   
7. Буруу JSON: Формат нь эвдэрсэн JSON өгөгдөл илгээх $\rightarrow$ Статус 400, result: "ERROR_BAD_JSON".   
8. Нөхцөлгүй хичээл: Prerequisites нь хоосон [] байх үед амжилттай бүртгэх $\rightarrow$ Статус 201, result: "OK".   

