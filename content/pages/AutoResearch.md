# AutoResearch (Andrej Karpathy)
#AI #AgenticAI #AutoResearch #RecursiveSelfImprovement #Optimization

**Source:** [YouTube - David Ondrej](https://youtu.be/uBWuKh1nZ2Y)

## Overview
AutoResearch คือระบบ Open-source โดย Andrej Karpathy ที่ช่วยให้ AI Agent สามารถพัฒนาตัวเองหรือโปรเจกต์ต่างๆ ได้อย่างอัตโนมัติ ผ่านลูปการทดลอง (Experimental Loop) โดยใช้หลักการวิทยาศาสตร์: ตั้งสมมติฐาน $\rightarrow$ ทดลอง $\rightarrow$ วัดผล $\rightarrow$ ปรับปรุง

## The AutoResearch Loop
กระบวนการทำงานเป็นวงจรปิด (Closed Loop) ดังนี้:
1. **Hypothesis:** Agent วิเคราะห์และเสนอแนวทางปรับปรุง
2. **Modification:** แก้ไขโค้ดในไฟล์ที่กำหนด
3. **Evaluation:** รันการทดสอบด้วย Metric ที่กำหนดไว้ในเวลาที่จำกัด (Time-boxed) เพื่อให้ทุกการทดลองเปรียบเทียบกันได้
4. **Persistence:** 
   - หากผลลัพธ์ดีขึ้น $\rightarrow$ **Commit** ลงใน Git history
   - หากผลลัพธ์แย่ลง $\rightarrow$ **Git Reset** กลับไปจุดเดิมแล้วเริ่มลูปใหม่

## Critical Architecture (The 3-File System)
เพื่อให้ระบบทำงานได้อย่างเที่ยงตรง ต้องมีโครงสร้างไฟล์ดังนี้:
- **`program.md`**: ไฟล์คำสั่งและเป้าหมายที่มนุษย์เป็นคนเขียน (The Goal)
- **`train.py` (Editable File)**: ไฟล์เดียวที่ Agent มีสิทธิ์แก้ไข เพื่อทดลองเปลี่ยนค่าหรือ Logic
- **`prepare.py` (The Judge)**: สคริปต์วัดผลที่ **ห้าม AI แก้ไขเด็ดขาด** เพื่อป้องกันไม่ให้ AI เขียนโค้ดโกงคะแนน (Cheat the eval)

## Success Conditions
การจะทำ AutoResearch ให้สำเร็จ ต้องมี 3 องค์ประกอบนี้ครบถ้วน:
1. **Clear Metric:** มีตัวเลขชี้วัดตัวเดียวที่ชัดเจน (เช่น Loading Time, Accuracy, Profit)
2. **Automated Eval:** การวัดผลต้องเป็นอัตโนมัติ 100% ไม่ต้องรอคนกดยืนยัน
3. **Single Point of Change:** มีไฟล์ที่แก้ไขได้เพียงไฟล์เดียว เพื่อให้รู้ว่าการเปลี่ยนแปลงนั้นส่งผลต่อ Metric อย่างไร

## Applications
- **Trading:** ปรับแต่งกฎ Buy/Sell โดยวัดจาก Sharpe Ratio
- **Marketing:** ปรับแก้ Copywriting ของโฆษณา โดยวัดจาก Conversion Rate
- **Software Dev:** ปรับปรุงความเร็วของ Website (เช่น ลด Load time จาก 50ms $\rightarrow$ 25ms)
- **Prompt Engineering:** ปรับปรุง System Prompt ของ Agent โดยวัดจาก Benchmark ของงานนั้นๆ

---
*Linked to: [[Harness Engineering]], [[AI Agents]], [[Recursive Self-Improvement]]*
