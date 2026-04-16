# Markdown is not the answer for AI Agents
#AI #AgenticAI #ContextWindow #TokenOptimization #HarnessEngineering

**Author:** Shane Murphy
**Source:** [YouTube](https://youtu.be/y1NBC1iXiL4)

## Overview
วิดีโอนี้นำเสนอแนวคิดที่ว่า การใช้ Markdown เป็นรูปแบบหลักในการส่ง Context ให้ AI Agent อาจไม่ใช่ทางออกที่ดีที่สุด แม้ว่าจะเป็นที่นิยมที่สุดในปัจจุบัน แต่ปัจจัยที่ส่งผลต่อความฉลาดของ Agent จริงๆ คือวิธีการที่เราโครงสร้าง (Structure), ใช้งาน (Use), และส่งมอบ (Deliver) Context นั้นๆ

## Key Insights

### 1. The Context Window Paradox
- **Efficiency Threshold:** LLM ทำงานได้มีประสิทธิภาพสูงสุดเมื่อใช้ Token ประมาณ **20%** ของขนาด Context Window เท่านั้น
- **Performance Decay:** เมื่อมีการใช้ Token มากขึ้นเรื่อยๆ ประสิทธิภาพของโมเดลจะลดลงอย่างเห็นได้ชัด (Steep decline) แม้ว่าโมเดลรุ่นใหม่ๆ จะมี Context Window ขนาดใหญ่มาก (เช่น 1-2 ล้าน Token) แต่ก็ไม่ได้หมายความว่าจะใช้งานได้เต็มประสิทธิภาพตลอดทั้ง Window

### 2. Format Analysis (Markdown vs Others)
- **Markdown:** เป็นรูปแบบอันดับ 1 สำหรับการทำ Retrieval และ Feed context เพราะอ่านง่ายสำหรับมนุษย์และ LLM
- **XML & JSON:** JSON มีข้อเสียคือการใช้ Token ซ้ำซ้อนในส่วนของ Label และ Schema ทำให้สิ้นเปลือง Token โดยไม่จำเป็น
- **Token Efficiency:** การลดจำนวน Token (เช่นการใช้เครื่องมืออย่าง Tune เพื่อลด Clutter ใน JSON) ช่วยเรื่องความประหยัด แต่ **ไม่ได้เพิ่มความแม่นยำ (Accuracy)** ให้เหนือกว่า Markdown

### 3. The Underused Tool: The '@' Symbol
- สัญลักษณ์ `@` มีประสิทธิภาพสูงมากในการระบุความสัมพันธ์ (Relations) ระหว่าง "เจตนา" (Intent) กับ "ข้อมูล" (File/Object) ใน Context (คล้ายกับการ Mention ใน Slack) ซึ่งเป็นสิ่งที่ Harness ส่วนใหญ่ยังไม่ได้นำมาใช้ประโยชน์อย่างเต็มที่

### 4. Shift to Harness Engineering
- ปัญหาที่แท้จริงไม่ใช่เรื่องของขนาด Window หรือการลด Token แต่เป็นเรื่องของ **Intent, Purpose, และ Productivity**
- เราต้องเปลี่ยนวิธีคิดจากการ "เพิ่มขนาดท่อ" (Context Window) เป็นการ "ปรับปรุงวิธีการส่งน้ำ" (Harnessing) เพื่อให้ Agent ทำงานได้ยาวนาน (Long horizon tasks) โดยยังอยู่ในจุดที่โมเดลทำงานได้มีประสิทธิภาพสูงสุด (~20% marker)

---
*Linked to: [[Harness Engineering]], [[AI Agents]], [[Context Window]]*
