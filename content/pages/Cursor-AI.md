# 💻 Cursor AI

**Cursor AI** เป็น AI-powered IDE (fork จาก VS Code) ที่ถูกออกแบบมาเพื่อการพัฒนาซอฟต์แวร์โดยมี AI เป็นส่วนประกอบหลัก (AI-first Code Editor)

## 🚀 Pro Prompting Strategies
*อ้างอิงจาก [[cursor-102-pro-prompting]]*

### 1. Context & Cost Management (Indie Dev Style)
- **Specific Refs (@file):** ให้ระบุไฟล์เจาะจงแทนการใช้ `@codebase` ทั้งหมด เพื่อลด Token Burn และเสียงรบกวน (Noise)
- **Concise Response:** สั่งให้ AI ตอบกระชับ (เช่น "max 4 sentences") และข้าม "Chit-chat" เพื่อเน้นความเร็วและประหยัด
- **.cursorrules:** ใช้ไฟล์กฎถาวรเพื่อเก็บสไตล์และสถาปัตยกรรม ช่วยประหยัดได้ 300-800 tokens ต่อการ Prompt

### 2. Strategic Workflows
- **Q&A Strategy:** ก่อนให้ AI เขียนโค้ดซับซ้อน (เช่น Stripe, DB Design) ให้สั่งว่า "Ask me 2-3 clarifying questions first" เพื่อลดความผิดพลาด
- **Plan mode:** บังคับให้ AI ทำลำดับขั้นตอน (Numbered Plan) และรอเราอนุมัติ (Approval) ก่อนเริ่มลงมือเขียนโค้ดจริง
- **Incremental Loop:** แบ่งงานใหญ่เป็นก้อนเล็ก (Skeleton -> Logic -> UI) เพื่อให้ AI ทำงานได้แม่นยำขึ้นและตรวจแก้ง่าย

## 🔗 Related Concepts
- [[llm-wiki-concept]]
- [[Andrej Karpathy]] (หนึ่งในผู้สนับสนุนหลักของ Cursor)

---
*Created by Anya (AI Secretary) during Wiki Ingestion on 2026-04-14*
