# 🌐 Publishing Wiki (wiki.powpoy.com)

แนวทางการนำ Obsidian Vault ไปแสดงผลเป็นเว็บไซต์สาธารณะ พร้อมฟีเจอร์ Graph View บนโดเมน `wiki.powpoy.com`

## 🛠️ เครื่องมือที่แนะนำ: Quartz (v4)
อ้างอิงจาก [[quartz-obsidian-website]] (zGFroBGud7w) เป็นทางเลือกที่ดีที่สุดสำหรับสาย Open-source เพราะรองรับ Graph View แบบ Interactive และปรับแต่งได้สูง

### 📋 ลำดับขั้นตอนการติดตั้ง (Step-by-Step)

#### 1. การเตรียมสภาพแวดล้อม (Local Setup)
- ติดตั้ง **Node.js** (v18.14.x ขึ้นไป)
- ติดตั้ง **Git**
- ใช้คำสั่งเพื่อ Clone Quartz:
  ```bash
  git clone https://github.com/jackyzha0/quartz.git
  cd quartz
  npm install
  npx quartz create  # เลือก 'Link your Obsidian Vault'
  ```

#### 2. การกำหนดค่าโดเมน (Domain Setup: wiki.powpoy.com)
- ในไฟล์ `quartz.config.ts` ให้แก้ไขส่วน `baseUrl` เป็น `wiki.powpoy.com`
- ตั้งค่า **CNAME** ใน DNS Provider ของพี่เอิบ (เช่น Cloudflare/GoDaddy) ให้ชี้ไปยัง GitHub Pages (เช่น `username.github.io`)

#### 3. การแสดงผล Graph View
- Quartz v4 มี Component `Graph` มาให้ในตัว
- สามารถปรับแต่งสี แรงดึงดูด (Repulsion) และการเชื่อมโยงได้ใน `quartz.layout.ts`

#### 4. การ Deploy (GitHub Pages)
- สร้าง Repository ใหม่บน GitHub
- ใช้คำสั่ง `npx quartz sync` เพื่อ Push ข้อมูลจากเครื่องขึ้น GitHub
- ระบบจะสร้างหน้าเว็บและโฮสต์ให้โดยอัตโนมัติ

## 🚀 แผนงานของอัญญา
- อัญญาจะช่วยรักษาโครงสร้างไฟล์ใน `/data/.openclaw/workspace/wiki/` ให้เป็นระเบียบ
- เมื่อพี่เอิบต้องการอัปเดตเว็บ อัญญาจะช่วยตรวจสอบความพร้อมของไฟล์ (Linting) ก่อนพี่เอิบจะรัน `npx quartz sync` ค่ะ

---
*Created by Anya (AI Secretary) on 2026-04-14*
