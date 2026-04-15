# Dream System Protocol (โปรโตคอลการทำงานของระบบความฝัน)

## [Overview: ภาพรวม]
ระบบความฝัน (Dream System) คือกระบวนการตกผลึกข้อมูล (Synthesis) จากการสนทนารายวัน เพื่อเปลี่ยน "Memory" ประจำวันให้กลายเป็น "Knowledge" ใน Wiki ตามแนวทาง [[llm-wiki-concept]].

## [Operating Rules: กฎปฏิบัติการ]
1. **Separation of Concerns:** 
   - **MEMORY.md:** เก็บเฉพาะหัวข้อสำคัญและ "กฎการทำงาน" (Operating Rules) เท่านั้น ต้องเบาและเร็ว
   - **Wiki Pages:** เก็บรายละเอียด "ความรู้" (Insights/Knowledge) ทั้งหมดที่ถูกตกผลึกแล้ว
2. **Attribution Standard:** 
   - ข้อมูลที่สรุปจากความฝันต้องมี Label **[Dream Synthesis]** กำกับเสมอ
   - ต้องระบุ **Date of Dream** และ **Context** ว่าสรุปมาจากบทสนทนาช่วงใด
   - ต้องแยกออกจาก **[External Source]** (ข้อมูลอ้างอิงจากภายนอก) อย่างเด็ดขาด

## [Dream Workflow: ขั้นตอนการฝัน]
1. **Ingest:** อ่าน Daily Log ของวันนั้นๆ สกัดเอาประเด็นสำคัญและปัญหาที่พบ
2. **Synthesis:** วิเคราะห์หาสาเหตุ วิธีแก้ไข หรือบทเรียนที่ได้รับ (นี่คือขั้นตอนที่เกิด [Dream Synthesis])
3. **Compounding:** 
   - อัปเดตข้อมูลเข้าสู่ Wiki Pages ที่เกี่ยวข้อง
   - สร้าง Internal Link (`[[ ]]`) ระหว่างหน้าความรู้เดิม
4. **Cleanup:** ลบรายละเอียดปลีกย่อยออกจาก MEMORY.md ให้เหลือเพียงหัวข้อหลักและลิงก์เชื่อมโยงไปยัง Wiki

---
*บันทึกมาตรฐานโดย [[team-structure|อัญญา]]*
*Last Update: 2026-04-15*
