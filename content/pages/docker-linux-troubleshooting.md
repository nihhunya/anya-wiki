# 🛠️ การแก้ไขปัญหา OpenClaw บน Docker (Linux)
#docker #linux #openclaw #troubleshooting #permissions #dns #networking

*สรุปแนวทางแก้ไขปัญหาการติดตั้งและการรัน OpenClaw บนสภาพแวดล้อม Docker บน Linux เพื่อให้ระบบทำงานได้อย่างเสถียร*

## 📌 สรุปปัญหาและแนวทางแก้ไข

### 1. ปัญหา Permission Denied (`/data`) #permissions
**อาการ:** พบ Error `EACCES: permission denied, mkdir '/data'` ใน Log และไม่สามารถส่งข้อความผ่าน LINE ได้
**สาเหตุ:** OpenClaw พยายามสร้างโฟลเดอร์ที่ Root (`/data`) ของ Container แต่รันด้วย User `node` (UID 1000) ซึ่งไม่มีสิทธิ์เขียนไฟล์ที่ Root
**วิธีแก้ไข:** ใช้เทคนิค **Bind Mount** ใน `docker-compose.yml` เพื่อ Map โฟลเดอร์ที่ User มีสิทธิ์เข้าถึงข้างนอก ให้เป็น `/data` ภายใน Container
- **Config:** `- ./data/.openclaw/devices:/data` ในส่วนของ volumes

### 2. ปัญหา DNS และการเชื่อมต่อ (Networking) #networking #dns
**อาการ:** `DNS lookup for the provider endpoint failed` หรือหา Ollama ไม่เจอ
**สาเหตุ:** 
- Linux ไม่รู้จัก `host.docker.internal` โดยอัตโนมัติเหมือน Windows/Mac
- Container ขาดการตั้งค่า DNS สำหรับ Resolve ชื่อโดเมนภายนอก (OpenRouter/Gemini)
**วิธีแก้ไข:**
- เพิ่ม `extra_hosts` ใน `docker-compose.yml` เพื่อชี้ `host.docker.internal` ไปที่ Gateway ของ Docker
- กำหนด `dns: [8.8.8.8]` ใน `docker-compose.yml` เพื่อให้ออกอินเทอร์เน็ตได้แน่นอน

### 3. ปัญหา CLI Plugin Blocked #sandbox #plugins
**อาการ:** คำสั่งเช่น `chat` หรือ `pair` ถูกปฏิเสธด้วยข้อความ `plugins.allow excludes "chat"`
**สาเหตุ:** ระบบ Sandbox ของ OpenClaw ปิดฟีเจอร์ Command Line ไว้เป็นค่าเริ่มต้นเพื่อความปลอดภัย

---

## 📋 ตารางสรุปการตั้งค่าที่แนะนำ (Golden Config) #config

| จุดที่แก้ไข | ไฟล์ | ค่าที่ตั้งค่า | ผลลัพธ์ |
| :--- | :--- | :--- | :--- |
| **Path Mapping** | `docker-compose.yml` | `- ./data/.openclaw/devices:/data` | แก้ปัญหา Permission Denied |
| **Network Host** | `docker-compose.yml` | `extra_hosts: host-gateway` | เชื่อมต่อ Ollama (Local) ได้ |
| **Internet DNS** | `docker-compose.yml` | `dns: 8.8.8.8` | เชื่อมต่อ API ภายนอกได้ |
| **File Ownership** | Terminal | `chown -R 1000:1000 ./data` | ระบบอ่าน/เขียน Config ได้ถูกต้อง |

## 💡 บทเรียนสำคัญ (Key Takeaway) #lesson-learned
จุดวิกฤตของการรัน OpenClaw บน Linux คือเรื่อง **User 1000 (Permissions)** และการจัดการ **Volume Mapping** การเตรียม Folder และ Map Volume ให้ถูกต้องตั้งแต่วันแรกจะช่วยลดปัญหาเรื่องสิทธิ์การเขียนไฟล์ได้อย่างมาก

---
**แหล่งอ้างอิง:** [[karpathy-llm-wiki-guide]], [[raw-sources/solved260415.md]]
**สถานะ:** ✅ แก้ไขสำเร็จ
