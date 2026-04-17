# Harness Engineering (วิศวกรรมฮาร์เนส)

**Harness Engineering** คือศาสตร์ของการออกแบบและสร้างระบบ (Infrastructure) ที่ล้อมรอบ AI Agent เพื่อเปลี่ยนให้โมเดลที่ทรงพลังแต่คาดเดาไม่ได้ กลายเป็นเครื่องมือที่ทำงานได้อย่างแม่นยำและเชื่อถือได้ในระดับ Production โดยทำหน้าที่เป็น [[Orchestration Layer]] ที่ควบคุมพฤติกรรมของโมเดล

## 核心 Concept: The Horse Metaphor 🐴
- **The Horse (ม้า):** คือ AI Model (ทรงพลัง รวดเร็ว แต่ไม่รู้ทิศทาง)
- **The Harness (ฮาร์เนส/สายบังเหียน):** คือ Infrastructure, Constraints, Feedback Loops และ Documentation ที่คอยควบคุมทิศทาง
- **The Rider (คนขี่):** คือ Human Engineer ที่คอยกำหนดทิศทางและให้คำแนะนำ

> "Model คือสินค้าโภคภัณฑ์ (Commodity) แต่ Harness คือป้อมปราการ (Moat)"

## 3 เสาหลักของ Harness Engineering 🏛️
1. **Context Engineering (วิศวกรรมบริบท):** การทำให้ Agent มีข้อมูลที่ถูกต้องในเวลาที่เหมาะสม ซึ่งเป็นส่วนหนึ่งของ [[LLM Optimization]]
   - *Static Context:* เอกสารสถาปัตยกรรม, `AGENTS.md`, Style Guides
   - *Dynamic Context:* Log, Metrics, โครงสร้าง Directory ปัจจุบัน
2. **Architectural Constraints (ข้อจำกัดทางสถาปัตยกรรม):** การบังคับใช้กฎเชิงกลไกแทนการใช้ Prompt เพื่อเพิ่มความเสถียรให้ [[AI Agents]]
   - การกำหนด Layer ของ Dependency (เช่น Types $\rightarrow$ Config $\rightarrow$ Repo $\rightarrow$ Service)
   - ใช้ Deterministic Linters และ LLM-based Auditors ในการตรวจสอบ
3. **Entropy Management (การจัดการเอนโทรปี):** การทำความสะอาดระบบเพื่อป้องกันข้อมูล drift
   - การใช้ Cleanup Agents ตรวจสอบความสอดคล้องของเอกสารและโค้ดอย่างสม่ำเสมอ

## การเปลี่ยนแปลงบทบาทของ Software Engineer 🛠️
| ก่อนมี Harness | หลังมี Harness |
| :--- | :--- |
| เขียนโค้ด (Write Code) | ออกแบบสภาพแวดล้อมให้ AI เขียนโค้ด |
| ดีบั๊กโค้ด (Debug Code) | ดีบั๊กพฤติกรรมของ Agent |
| รีวิวโค้ด (Review Code) | รีวิว Output ของ Agent + ประสิทธิภาพของ Harness |
| เขียน Test | ออกแบบกลยุทธ์การ Test ให้ Agent รัน |
| เขียนเอกสาร | สร้าง Documentation เป็น Infrastructure ที่เครื่องอ่านได้ |

## แนวทางปฏิบัติ (Practical Framework)
- **Level 1 (Basic):** ใช้ `.cursorrules` หรือ `CLAUDE.md`, ทำ Pre-commit hooks, มี Test suite
- **Level 2 (Team):** ใช้ `AGENTS.md` ร่วมกัน, บังคับ Constraints ผ่าน CI, ใช้ Prompt templates
- **Level 3 (Production):** ใช้ Middleware (Loop detection, Reasoning sandwich), เชื่อมต่อ Observability, มี Entropy agents

---
**แหล่งอ้างอิง:** [[harness-engineering-guide|Raw Source]]
