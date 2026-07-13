# บทที่ 1: รู้จัก Spring Boot

## Spring Boot คืออะไร?

Spring Boot คือ framework สำหรับสร้างแอปพลิเคชัน Java (เช่น REST API, Web App)
ที่ **ตั้งค่าให้เกือบทุกอย่างอัตโนมัติ** — ไม่ต้องเขียน config XML เยอะ ๆ เหมือน Spring แบบเก่า

จุดเด่น:

- ✅ **Auto-configuration** — ตั้งค่าให้เองตาม dependency ที่เราใส่
- ✅ **Embedded Server** — มี Tomcat ในตัว รันได้เลยด้วย `java -jar`
- ✅ **Starter Dependencies** — ใส่ dependency เดียว ได้ของครบชุด เช่น `spring-boot-starter-web`

## เวอร์ชันปัจจุบัน (ก.ค. 2026)

| สาย | Patch ล่าสุด | Java ขั้นต่ำ | ซัพพอร์ตถึง | เหมาะกับ |
|---|---|---|---|---|
| **4.1** | 4.1.0 | Java 17 | ก.ค. 2027 | โปรเจกต์ใหม่ อยากได้ฟีเจอร์ล่าสุด |
| **4.0** | 4.0.7 | Java 17 | ธ.ค. 2026 | — |
| **3.5 (LTS)** | 3.5.16 | Java 17 | มิ.ย. 2032 | โปรเจกต์ที่ต้องการความเสถียรยาว ๆ |

**ฟีเจอร์เด่นของ 4.x:**
- รองรับ **gRPC** ในตัว (4.1)
- **HTTP Service Clients** — ประกาศ interface แล้ว Spring ยิง REST ให้เอง (4.0)
- **API Versioning** ในตัว (4.0)
- **OpenTelemetry starter** สำหรับ metrics/tracing (4.0)
- ป้องกัน **SSRF** ด้วย `InetAddressFilter` (4.1)


---

[🏠 สารบัญ](../README.md) | [บทที่ 2: Flow การทำงาน](02-how-it-works.md) ➡️
