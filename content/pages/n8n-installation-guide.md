# n8n Installation Guide (ฉบับอัญญา)

คู่มือการติดตั้ง n8n ในรูปแบบที่อัญญาดูแลได้ โดยเน้นความง่ายในการจัดการและมีความคงทนของข้อมูล

## 🛠️ Micro-steps สำหรับการติดตั้ง

### Step 1: เตรียมพื้นที่เก็บข้อมูล (Data Persistence)
สร้างโฟลเดอร์สำหรับเก็บฐานข้อมูลและคอนฟิก เพื่อป้องกันข้อมูลสูญหาย
\`\`\`bash
mkdir -p /home/node/.openclaw/workspace/n8n-data
\`\`\`

### Step 2: ติดตั้ง n8n ผ่าน npm
ติดตั้งตัวโปรแกรม n8n ลงในระดับ Global ของระบบ
\`\`\`bash
npm install -g n8n
\`\`\`

### Step 3: การรัน n8n ครั้งแรก (Initial Run)
รัน n8n โดยกำหนดให้เก็บข้อมูลไว้ในโฟลเดอร์ที่เราเตรียมไว้
\`\`\`bash
export N8N_USER_FOLDER=/home/node/.openclaw/workspace/n8n-data
n8n start
\`\`\`
*(หมายเหตุ: เมื่อรันแล้ว ให้สังเกต URL ที่ปรากฏใน Terminal เพื่อเข้าใช้งานหน้า Dashboard)*

### Step 4: ทำให้ n8n ทำงานตลอดเวลา (Background Process)
แนะนำให้ใช้ `pm2` ในการจัดการเพื่อให้ n8n รันอยู่เบื้องหลังและ restart ตัวเองอัตโนมัติหากเกิด Crash
\`\`\`bash
# ติดตั้ง pm2
npm install -g pm2

# เริ่มรัน n8n ด้วย pm2
N8N_USER_FOLDER=/home/node/.openclaw/workspace/n8n-data pm2 start n8n --name "n8n-anya" -- start

# บันทึกสถานะเพื่อให้รันตอนเปิดเครื่อง
pm2 save
\`\`\`

### Step 5: การเข้าใช้งานและตรวจสอบ
- **URL:** เข้าผ่าน `http://<IP-VPS>:5678`
- **ตรวจสอบ Log:** `pm2 logs n8n-anya`
- **สั่ง Restart:** `pm2 restart n8n-anya`

---
**ผู้บันทึก:** [[Anya]]
**วันที่:** 2026-04-17
