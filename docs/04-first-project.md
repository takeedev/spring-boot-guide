# บทที่ 4: เริ่มโปรเจกต์แรก


1. ไปที่ **[start.spring.io](https://start.spring.io)** (Spring Initializr)
2. เลือก:
   - Project: **Maven** หรือ Gradle
   - Language: **Java**
   - Spring Boot: **4.1.0** (หรือ 3.5.x ถ้าอยากได้ LTS)
   - Java: **17** ขึ้นไป (แนะนำ 21)
3. เพิ่ม Dependencies พื้นฐาน:
   - `Spring Web` — สร้าง REST API
   - `Spring Data JPA` — คุยกับ database
   - `H2 Database` — database ในหน่วยความจำ ไว้ฝึก
   - `Validation` — ตรวจสอบข้อมูล
4. กด **Generate** → แตก zip → เปิดใน IDE → รัน!

```bash
./mvnw spring-boot:run
# แอปจะรันที่ http://localhost:8080
```


---

⬅️ [บทที่ 3: Annotation พื้นฐาน](03-annotations.md) | [🏠 สารบัญ](../README.md) | [บทที่ 5: JPA Relationships](05-jpa-relationships.md) ➡️
