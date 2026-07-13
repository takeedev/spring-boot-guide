# 📘 Spring Boot Guide สำหรับผู้เริ่มต้น

> คู่มือ Spring Boot ภาษาไทย อ่านจบทำงานได้จริง — เรียงเป็นบทจากง่ายไปยาก พร้อมตัวอย่างโค้ดและ diagram ทุกบท
>
> อัปเดตล่าสุด: กรกฎาคม 2026 | อิงเวอร์ชัน: **Spring Boot 4.1.0**

---

## 📖 สารบัญ

### ส่วนที่ 1: พื้นฐาน — เริ่มจากศูนย์

| บท | เนื้อหา | เหมาะกับ |
|---|---|---|
| [บทที่ 1: รู้จัก Spring Boot](docs/01-what-is-spring-boot.md) | Spring Boot คืออะไร จุดเด่น และเวอร์ชันปัจจุบัน | ยังไม่เคยแตะเลย |
| [บทที่ 2: Flow การทำงาน](docs/02-how-it-works.md) | Request วิ่งผ่าน Controller → Service → Repository ยังไง | อยากเห็นภาพรวมก่อนเขียนโค้ด |
| [บทที่ 3: Annotation พื้นฐาน](docs/03-annotations.md) | Annotation ทุกตัวที่ต้องรู้ แยกตาม layer พร้อมตัวอย่าง | บทหลักของ repo นี้ 📌 |
| [บทที่ 4: เริ่มโปรเจกต์แรก](docs/04-first-project.md) | สร้างโปรเจกต์จาก start.spring.io แล้วรันให้ขึ้น | พร้อมลงมือ |

### ส่วนที่ 2: ลงลึกทีละเรื่อง

| บท | เนื้อหา | เหมาะกับ |
|---|---|---|
| [บทที่ 5: JPA Relationships](docs/05-jpa-relationships.md) | @OneToMany, @ManyToOne, foreign key, LAZY/cascade | เริ่มมีหลายตาราง |
| [บทที่ 6: การเขียน Test](docs/06-testing.md) | @WebMvcTest, @DataJpaTest, @Mock vs @MockitoBean | อยากเทสเป็น |
| [บทที่ 7: Spring Security](docs/07-security.md) | Authentication vs Authorization, 401 vs 403 | ต้องมี login/สิทธิ์ |
| [บทที่ 8: เจาะลึก Bean](docs/08-bean-deep-dive.md) | IoC, lifecycle, singleton, @Qualifier/@Primary | อยากเข้าใจว่า Spring ทำงานยังไงจริง ๆ |
| [บทที่ 9: เจาะลึก @Transactional](docs/09-transactional.md) | commit/rollback, กลไก proxy และกับดัก self-invocation | เริ่มแตะ database จริงจัง |

### ส่วนที่ 3: สู่งานจริง

| บท | เนื้อหา | เหมาะกับ |
|---|---|---|
| [บทที่ 10: จากโค้ดฝึกหัดสู่งานจริง](docs/10-production-ready.md) | DTO, PostgreSQL + Flyway, Profiles, Logging, Lombok | ก่อนเข้าทีมจริง 📌 |
| [บทที่ 11: สิ่งที่เจอบ่อยในงานจริง](docs/11-real-world-essentials.md) | Pagination, JWT, Swagger, RestClient, Actuator | ทำ API ให้คนอื่นใช้ |
| [บทที่ 12: Annotation เพิ่มพลัง](docs/12-power-annotations.md) | @Scheduled, @Async, @Cacheable, @EventListener | เพิ่มความสามารถให้แอป |

### 🗂️ เปิดดูเร็ว ๆ

- [Cheat Sheet — สรุป Annotation ทั้งหมดในหน้าเดียว](docs/cheat-sheet.md)

---

## 🧭 อ่านยังไงดี?

- **มือใหม่หมด** → ไล่ตามลำดับ บทที่ 1 → 4 แล้วลงมือสร้างโปรเจกต์เลย จากนั้นค่อยกลับมาอ่านบทถัดไปตอนเจอเรื่องนั้นจริง
- **พอมีพื้นแล้ว** → ข้ามไปส่วนที่ 2 ได้เลย แนะนำบทที่ 8–9 เป็นพิเศษ เพราะอธิบายกลไกเบื้องหลัง (Bean, proxy) ที่ทำให้เข้าใจ annotation ตัวอื่นทั้งหมด
- **กำลังจะเข้าทีมจริง** → บทที่ 10 คือช่องว่างระหว่างโค้ดฝึกหัดกับโค้ดบริษัท อ่านก่อนวันแรกได้ยิ่งดี

> 💡 ทุกบทมี link ⬅️ ➡️ ท้ายหน้า อ่านต่อเนื่องได้เหมือนหนังสือ

---

## 📚 แหล่งอ้างอิง

- [Spring Boot Official Docs](https://docs.spring.io/spring-boot/index.html)
- [Spring Boot 4.1 Release Notes](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-4.1-Release-Notes)
- [Spring Initializr — สร้างโปรเจกต์](https://start.spring.io)
- [Baeldung — บทความสอน Spring ที่ดีที่สุด](https://www.baeldung.com/spring-boot)
- [Spring Security Docs](https://docs.spring.io/spring-security/reference/index.html)
- [Spring Boot Testing Docs](https://docs.spring.io/spring-boot/reference/testing/index.html)
