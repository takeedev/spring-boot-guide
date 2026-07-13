# บทที่ 2: Flow การทำงานของ Spring Boot

## Flow การทำงานของ Spring Boot (ภาพรวม)

เมื่อมี HTTP Request เข้ามา จะวิ่งผ่านชั้นต่าง ๆ แบบนี้:

```mermaid
flowchart TD
    Client["🌐 Client<br/>(Browser / Postman)"]
    DS["DispatcherServlet<br/><small>ด่านแรก รับทุก request แล้วหาว่าต้องส่งไป Controller ไหน</small>"]
    C["Controller — @RestController<br/><small>รับ request / ส่ง response</small>"]
    S["Service — @Service<br/><small>business logic เช่น คำนวณ ตรวจสอบเงื่อนไข</small>"]
    R["Repository — @Repository<br/><small>คุยกับฐานข้อมูล</small>"]
    DB[("🗄️ Database")]

    Client -->|"HTTP Request เช่น GET /api/users/1"| DS
    DS --> C
    C --> S
    S --> R
    R --> DB
    DB -.->|ผลลัพธ์ส่งกลับทีละชั้น| Client
```

> 💡 จำง่าย ๆ: **Controller = ประตูหน้าบ้าน, Service = สมอง, Repository = คนคุยกับ DB**

## Flow สรุป: จาก Request → Response ครบวงจร

ตัวอย่าง: ผู้ใช้ยิง `POST /api/users` พร้อม JSON

```mermaid
sequenceDiagram
    autonumber
    participant Client as 🌐 Client
    participant DS as DispatcherServlet
    participant Ctrl as UserController
    participant Svc as UserService
    participant Repo as UserRepository
    participant DB as 🗄️ Database

    Client->>DS: POST /api/users<br/>{"name": "John", "email": "john@mail.com"}
    DS->>Ctrl: หา Controller ที่ match กับ path
    Note over Ctrl: @RequestBody แปลง JSON → object<br/>@Valid ตรวจสอบข้อมูล (ถ้าพัง → ตอบ 400 ทันที)
    Ctrl->>Svc: เรียก service
    Note over Svc: @Transactional เริ่ม transaction
    Svc->>Repo: save(user)
    Repo->>DB: INSERT INTO users ...
    DB-->>Repo: บันทึกสำเร็จ
    Repo-->>Svc: User (มี id แล้ว)
    Svc-->>Ctrl: User
    Ctrl-->>Client: 200 OK<br/>Spring แปลง object → JSON ให้อัตโนมัติ
```


---

⬅️ [บทที่ 1: รู้จัก Spring Boot](01-what-is-spring-boot.md) | [🏠 สารบัญ](../README.md) | [บทที่ 3: Annotation พื้นฐาน](03-annotations.md) ➡️
