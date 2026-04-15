# GOG CLI: OAuth Authentication Troubleshooting

## 🚨 Current Status & Issues (Recorded 2026-04-15)
จากความผิดพลาดในการต่ออายุ Token สำหรับบัญชี `nihhunya@gmail.com` และ `goerb5167@gmail.com` พบประเด็นสำคัญดังนี้:

### 1. Port Randomization & Timeout
- `gog login` เปิด Local Server ที่สุ่ม Port (เช่น 35117, 42655, 46839)
- Session บน Server (VPS) มักจะ Timeout เร็วกว่าที่ผู้ใช้จะส่ง Code กลับมาทาง LINE
- **Result:** แม้ผู้ใช้ส่ง Code ถูกต้อง แต่ Server มักปิดรับไปก่อน (No active session found)

### 2. Browser Security Policy (Google OAuth)
- การใช้ `browser control` (Headless/Automated browser) ของระบบเพื่อ Login แทนผู้ใช้ถูก Google บล็อกด้วยข้อความ **"This browser or app may not be secure"** (403: access_denied)
- **Result:** ระบบอัตโนมัติไม่สามารถเข้าสู่หน้า Login ของ Google ได้โดยตรง

### 3. Google Cloud Testing Mode (403 access_denied)
- หากบัญชีอีเมลไม่ถูกเพิ่มในรายชื่อ **Test Users** ของ OAuth Consent Screen จะไม่สามารถต่อ Token ได้

## 💡 Proposed Solutions for Future
1. **Listen Address Locking:** ใช้แฟลก `--listen-addr 127.0.0.1:XXXXX` เพื่อล็อกพอร์ตให้ตายตัวสำหรับหน้า Dashboard
2. **Reverse Proxy Bridge:** สร้างสะพานเชื่อมจาก VPS มายัง Public Web (ThaiDocs/Wiki) เพื่อให้คลิกจากภายนอกได้โดยตรง
3. **Session Persistence:** พัฒนาสคริปต์ที่รันเบื้องหลังเพื่อคงสถานะรับ Code ให้ยาวขึ้น

---
*บันทึกโดย [[team-structure|อัญญา]]*
*Linked Raw Source: N/A (Direct Conversation Logic)*
