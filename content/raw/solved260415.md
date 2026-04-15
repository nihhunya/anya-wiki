**"ความไม่ลงตัวระหว่างระบบภายในของโปรแกรม กับสภาพแวดล้อม Docker บน Linux"** ครับ

นี่คือสรุปเหตุการณ์และหมัดเด็ดที่ใช้แก้จนฟื้นครับ:

---

### 1. ปัญหาแรก: "อัญญาพูดไม่ได้" (Permission Denied)
* **อาการ:** ใน LINE ขึ้นว่า "Something went wrong" และ Log พ่น Error `EACCES: permission denied, mkdir '/data'` ซ้ำๆ
* **สาเหตุ:** ตัว OpenClaw ถูก Hardcode มาให้พยายามสร้างโฟลเดอร์ที่ Root (`/data`) ของ Container แต่ตัว Container รันด้วย User `node` (UID 1000) ซึ่งไม่มีสิทธิ์เขียนไฟล์ที่ Root ของระบบ
* **วิธีที่ "เกือบ" ได้ผล:** การแก้ `openclaw.json` และ `chown` สิทธิ์เป็น 1000 (ช่วยให้ไฟล์ในบ้านเขียนได้ แต่แก้เรื่องที่มันจะออกไปนอกบ้านไม่ได้)
* **วิธีที่แก้ได้จริง:** ใช้เทคนิค **Bind Mount** ใน `docker-compose.yml` โดยการหลอกระบบว่า `/data` ใน Container คือที่เดียวกับโฟลเดอร์ที่เราคุมได้ข้างนอก ทำให้มันเขียนไฟล์ลงไปได้สำเร็จ

### 2. ปัญหาที่สอง: "อัญญาหลงทาง" (DNS Lookup Failed)
* **อาการ:** อัญญาตอบ LINE ได้แล้ว แต่บ่นว่า `DNS lookup for the provider endpoint failed`
* **สาเหตุ:**
    1.  ใน Config ใช้ `host.docker.internal` ซึ่ง Linux ไม่รู้จัก (ปกติใช้ได้แค่บน Windows/Mac) ทำให้หา Ollama ไม่เจอ
    2.  Container ไม่มีค่า DNS ทำให้ Resolve ชื่อ OpenRouter หรือ Gemini API ไม่ได้
* **วิธีที่แก้ได้จริง:**
    1.  เพิ่ม `extra_hosts` ใน `docker-compose.yml` เพื่อชี้เป้า `host.docker.internal` ไปที่ Gateway ของ Docker
    2.  เพิ่ม `dns: [8.8.8.8]` เพื่อให้ Container ออกเน็ตได้ชัวร์ๆ

### 3. ปัญหาที่สาม: "อัญญาโดนขัง" (CLI Plugin Blocked)
* **อาการ:** เราพยายามจะใช้คำสั่ง `chat` หรือ `pair` ผ่าน Terminal แต่ระบบฟ้องว่า `plugins.allow excludes "chat"`
* **สาเหตุ:** เป็นระบบ Sandbox ของ OpenClaw ที่ปิดฟีเจอร์ Command Line ไว้โดย Default เพื่อความปลอดภัย
* **สถานะปัจจุบัน:** เราเลือกที่จะไม่แก้กุญแจทุกดอก (ไม่แก้ JSON เพิ่ม) แต่เน้นให้ระบบหลัก (LINE) ทำงานได้ก่อน ซึ่งตอนนี้ทำงานได้แล้ว

---

### 📋 ตารางสรุปการตั้งค่าที่ทำให้ระบบ "รอด"

| จุดที่แก้ | ไฟล์ | ค่าที่ใส่ | ผลลัพธ์ |
| :--- | :--- | :--- | :--- |
| **Path หลอก** | `docker-compose.yml` | `- ./data/.openclaw/devices:/data` | เลิกบ่นเรื่อง Permission |
| **Network** | `docker-compose.yml` | `extra_hosts: host-gateway` | คุยกับ Ollama ได้ |
| **Internet** | `docker-compose.yml` | `dns: 8.8.8.8` | คุยกับ OpenRouter ได้ |
| **Permissions** | Terminal | `chown -R 1000:1000 ./data` | ระบบอ่าน/เขียน Config ได้ |

---

### 💡 บทเรียนสำหรับครั้งหน้า
ถ้าพี่เอิบจะย้ายเครื่องหรือรันใหม่ **"จุดตาย"** ของ OpenClaw บน Linux คือ **User 1000** ครับ มันจะเข้มงวดเรื่องสิทธิ์มาก และมักจะแอบไปเขียนไฟล์ที่ `/data` เสมอ การเตรียมโฟลเดอร์และ Map Volume ให้ตรงตั้งแต่วันแรกจะช่วยให้ไม่เหนื่อยแบบวันนี้ครับ