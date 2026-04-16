# Harness Engineering

**Source:** [Rethinking AI Agents: The Rise of Harness Engineering](https://youtu.be/Xxuxg8PcBvc)
**Date:** 14 Apr 2026

## Overview
Harness Engineering คือศาสตร์ของการออกแบบและปรับปรุง **"Orchestration Code"** หรือโครงสร้างที่ห่อหุ้ม (Wrap) ตัว LLM ไว้ ซึ่งพบว่าตัว Harness นี้มีผลต่อประสิทธิภาพการทำงานของ AI Agent มากกว่าตัวโมเดลพื้นฐาน (Foundational Model) ในหลายกรณี

## Key Findings
- **The 6x Gap:** ประสิทธิภาพของ Agent สามารถแตกต่างกันได้ถึง 6 เท่า แม้จะใช้โมเดลตัวเดียวกัน แต่ใช้ Harness ต่างกัน
- **Efficiency vs Bloat:** โครงสร้างที่ซับซ้อนเกินไป (Bloated) ไม่ได้ช่วยให้งานสำเร็จมากขึ้น แต่กลับใช้ Compute Resource มากกว่าเดิมถึง 14 เท่า เมื่อเทียบกับโครงสร้างแบบ Stripped (เรียบง่าย)
- **Natural Language Representation:** การใช้ Natural Language ในการกำหนด Control Logic ของ Harness ให้ผลลัพธ์ที่ดีกว่าการใช้ Python code ที่เปราะบาง (Brite) โดยเพิ่มความแม่นยำจาก 30.4% เป็น 47.2%
- **Transferability:** Harness ที่ได้รับการ Optimize แล้วสามารถย้ายไปใช้กับโมเดลอื่นได้ (Reusable Asset) พิสูจน์ว่าความฉลาดของระบบอยู่ที่การวางโครงสร้างการทำงาน ไม่ใช่แค่ที่ตัวโมเดล
- **The Verifier Paradox:** การเพิ่มโมดูลตรวจสอบ (Verifier) ไม่ได้การันตีว่าผลลัพธ์จะดีขึ้นเสมอไป ในบางกรณีกลับทำให้ประสิทธิภาพลดลง (เช่นใน OSWorld)

## Related Research
- [[Natural-Language Agent Harnesses]] (Tsinghua University, March 2026)
- [[Meta-Harness]] (Stanford University, March 2026)

---
*Linked to: [[AI Agents]], [[LLM Optimization]], [[Orchestration Layer]]*
