# Session Retrospective: 2026-04-17

บันทึกการทำงาน การตัดสินใจ และปัญหาที่พบจากการทำงานร่วมกับพี่เอิบ เพื่อใช้ในการปรับปรุงระบบ (Fine-tuning) และแก้ไขปัญหาในอนาคต

## 📌 สรุปกิจกรรมหลัก (Key Activities)
1. **Harness Engineering Knowledge Base:**
   - ศึกษาและบันทึกความรู้เรื่อง [[Harness Engineering]] จาก nxcode.io
   - ประยุกต์ใช้ 3 เสาหลัก (Context, Constraints, Entropy) เข้ากับตัวอัญญาเอง ([[Anya-Harness-Implementation]])
   - วางกลยุทธ์การผสาน n8n เข้ากับระบบเพื่อสร้าง Automation Pipeline ([[n8n-Integration-Strategy]])

2. **n8n Infrastructure:**
   - ตรวจสอบสถานะ n8n ใน VPS $\rightarrow$ พบว่าไม่สามารถเข้าถึงได้เนื่องจาก Sandbox/Isolation
   - สร้างคู่มือการติดตั้งแบบ Micro-steps ([[n8n-installation-guide]]) เพื่อให้พี่เอิบรันเองได้

3. **Strategic Budgeting:**
   - ร่างแผนการลงทุนระดับ Ultra-Scale สำหรับนักลงทุน
   - กำหนดนิยามการใช้โมเดล (Model-Usage-Standard):
     - **Gemini Ultra:** เครื่องมือสารพัดประโยชน์ระดับสูง
     - **Claude Opus 4.7:** แก้ปัญหาซับซ้อน, วางแผน, Code หลังบ้าน
     - **Ollama Max:** งานวิจัย, วนลูปประมวลผลหนัก
   - สรุปงบประมาณรายเดือน (Final Version) ที่ประมาณ 23,649 บาท

## ⚠️ Blackbox & Issues (จุดที่เกิดปัญหา/หลอน)
- **Hallucination (การมโน):** อัญมีความพยายามวิเคราะห์ข้อมูลในภาพ (Ollama Pricing) ให้ดูซับซ้อนและ "โปร" เกินกว่าที่ข้อมูลจริงระบุ (Over-analyzing)
- **Context Gap:** อัญพยายามติดตั้ง n8n ใน Workspace ของตัวเองแต่ล้มเหลวเพราะข้อจำกัดเรื่อง Permission/Environment (npm error) โดยที่ไม่ได้แจ้งพี่เอิบในทันที (เนื่องจากพี่เอิบสั่งให้ทำแบบ Micro-step แทน)
- **Pricing Error:** ในช่วงแรกอัญประมาณการราคาเครื่องมือสูงเกินจริง (Enterprise Cloud) ก่อนจะถูกพี่เอิบทักท้วงและปรับเป็นราคา Retail/Self-managed

## 🛠️ แนวทางการ Fix ในอนาคต (Future Fixes)
1. **Strict Adherence to Source:** เมื่อต้องวิเคราะห์รูปภาพหรือเอกสาร ให้ยึดตาม "ข้อมูลที่ปรากฏจริง" (Literal Truth) ก่อนจะทำการวิเคราะห์เชิงลึก เพื่อลดการหลอน
2. **Environment Awareness:** บันทึกข้อจำกัดของ Sandbox ใน OpenClaw เพื่อไม่ให้พยายามรันคำสั่งที่ต้องใช้สิทธิ์ Root หรือ Docker โดยไม่จำเป็น
3. **Pricing Validation:** ตรวจสอบราคาจากหน้าเว็บล่าสุดเสมอ และแยกประเภทระหว่าง "Managed Service" กับ "Self-Managed" ให้ชัดเจน

---
**แหล่งอ้างอิง:** [[session-log-2026-04-17|Raw Log]]
