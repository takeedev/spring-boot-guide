# บทที่ 10: จากโค้ดฝึกหัดสู่งานจริง


## 1. DTO — อย่าส่ง Entity ออกไปตรง ๆ

**DTO (Data Transfer Object)** = class แยกต่างหากสำหรับรับ/ส่งข้อมูลกับภายนอก

ทำไมไม่ใช้ Entity ตรง ๆ?
- Entity มี field ที่ไม่ควรหลุดออกไป (เช่น password, ข้อมูลภายใน)
- ความสัมพันธ์ JPA ทำให้เกิด JSON loop ไม่รู้จบ (User → Order → User → ...)
- พอแก้โครงสร้างตาราง API พังตามทันที เพราะผูกติดกัน

```java
// Entity — ใช้ภายใน คุยกับ database เท่านั้น
@Entity
public class User {
    @Id private Long id;
    private String name;
    private String email;
    private String password;        // ⚠️ ห้ามหลุดออกไปเด็ดขาด
    @OneToMany(...) private List<Order> orders;
}

// Response DTO — ใช้ Java record สั้น ๆ เลือกเฉพาะ field ที่อยากให้เห็น
public record UserResponse(Long id, String name, String email) {

    // วิธี map ง่ายสุด: static factory method
    public static UserResponse from(User user) {
        return new UserResponse(user.getId(), user.getName(), user.getEmail());
    }
}

// Controller คืน DTO ไม่ใช่ Entity
@GetMapping("/{id}")
public UserResponse getUser(@PathVariable Long id) {
    return UserResponse.from(userService.findById(id));
}
```

> 💡 หลักจำ: **Entity อยู่กับ database, DTO อยู่กับ API** — ถ้าโปรเจกต์ใหญ่ขึ้นค่อยใช้ library อย่าง MapStruct ช่วย map อัตโนมัติ

## 2. ต่อ database จริง (PostgreSQL) + Flyway

H2 ใช้ฝึกได้ แต่งานจริงต้องต่อ database จริง:

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
</dependency>
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-database-postgresql</artifactId>
</dependency>
```

```properties
# application.properties
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}

# ⚠️ ห้ามใช้ ddl-auto=update ใน production — ให้ Flyway จัดการโครงสร้างตารางแทน
spring.jpa.hibernate.ddl-auto=validate
```

**Flyway** = ระบบจัดการโครงสร้างตารางแบบมีเวอร์ชัน — เขียน SQL เป็นไฟล์เรียงลำดับ
วางไว้ที่ `src/main/resources/db/migration/` แล้ว Flyway รันให้อัตโนมัติตอนแอปสตาร์ท:

```
db/migration/
├── V1__create_users_table.sql
├── V2__create_orders_table.sql
└── V3__add_email_index.sql       ← เพิ่มไฟล์ใหม่เมื่อต้องแก้โครงสร้าง ห้ามแก้ไฟล์เก่า
```

ข้อดี: ทุกเครื่อง (เพื่อนร่วมทีม, server ทดสอบ, production) มีโครงสร้างตารางตรงกันเสมอ และย้อนดูประวัติได้ว่าใครแก้อะไรเมื่อไหร่

## 3. Profiles — แยก config ตามสภาพแวดล้อม

งานจริงมีอย่างน้อย 3 สภาพแวดล้อม: เครื่องเรา (dev), เครื่องทดสอบ (staging), ของจริง (prod)

```
application.properties          ← ค่ากลางที่ใช้ร่วมกันทุกที่
application-dev.properties      ← ต่อ database ในเครื่อง, log ละเอียด
application-prod.properties     ← ต่อ database จริง, log เฉพาะจำเป็น
```

```properties
# application-dev.properties
spring.datasource.url=jdbc:postgresql://localhost:5432/mydb_dev
logging.level.com.example=DEBUG

# application-prod.properties
spring.datasource.url=${DB_URL}          # ⚠️ secret มาจาก environment variable เสมอ
logging.level.com.example=INFO
```

เลือก profile ตอนรัน:

```bash
java -jar app.jar --spring.profiles.active=prod
# หรือตั้ง env: SPRING_PROFILES_ACTIVE=prod
```

> ⚠️ กฎเหล็ก: **ห้าม hardcode password/secret ลงไฟล์ properties ที่ commit ขึ้น git** — ใช้ `${ENV_VAR}` ดึงจาก environment variable เสมอ

## 4. Logging — เลิกใช้ System.out.println

งานจริงใช้ **SLF4J** ซึ่ง Spring Boot ตั้งค่ามาให้แล้ว:

```java
@Service
public class OrderService {

    private static final Logger log = LoggerFactory.getLogger(OrderService.class);
    // หรือถ้าใช้ Lombok: ติด @Slf4j ที่ class แล้วใช้ log ได้เลย

    public Order placeOrder(OrderRequest request) {
        log.info("รับออเดอร์ใหม่ userId={}", request.userId());   // {} = placeholder
        try {
            ...
        } catch (PaymentException e) {
            log.error("ชำระเงินล้มเหลว orderId={}", order.getId(), e);  // ใส่ e ท้ายสุด ได้ stack trace
            throw e;
        }
    }
}
```

| ระดับ | ใช้เมื่อ |
|---|---|
| `log.debug` | รายละเอียดสำหรับ debug — ปิดใน prod |
| `log.info` | เหตุการณ์ปกติที่ควรบันทึก เช่น รับออเดอร์, จบงาน batch |
| `log.warn` | ผิดปกติแต่ยังทำงานต่อได้ |
| `log.error` | พังจนงานนั้นไม่สำเร็จ — ต้องมีคนมาดู |

ทำไมดีกว่า `System.out.println`: เลือกเปิด/ปิดตามระดับและ package ได้, มี timestamp ให้, ส่งเข้าระบบเก็บ log กลาง (เช่น ELK) ได้

## 5. Lombok — ลดโค้ด boilerplate

Lombok gen โค้ดซ้ำซากให้ตอน compile — เจอในโค้ดบริษัทแทบทุกที่:

| Annotation | gen อะไรให้ |
|---|---|
| `@Getter` / `@Setter` | getter/setter ทุก field |
| `@RequiredArgsConstructor` | constructor สำหรับ field ที่เป็น `final` — **คู่กับ constructor injection พอดี** |
| `@Builder` | สร้าง object แบบ `User.builder().name("John").build()` |
| `@Slf4j` | ตัวแปร `log` สำหรับ logging |
| `@Data` | Getter+Setter+equals+hashCode+toString (สะดวกแต่⚠️อย่าใช้กับ Entity — equals/hashCode ตีกับ JPA) |

```java
@Service
@RequiredArgsConstructor   // gen constructor ให้ → Spring ฉีด dependency ผ่านมันอัตโนมัติ
@Slf4j
public class UserService {

    private final UserRepository userRepository;   // ไม่ต้องเขียน constructor เองแล้ว!

    public User findById(Long id) {
        log.debug("หา user id={}", id);
        return userRepository.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));
    }
}
```


---

⬅️ [บทที่ 9: เจาะลึก @Transactional](09-transactional.md) | [🏠 สารบัญ](../README.md) | [บทที่ 11: สิ่งที่เจอบ่อยในงานจริง](11-real-world-essentials.md) ➡️
