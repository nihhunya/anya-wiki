# Gemma 4 (Google Open Models)

## Overview
Gemma 4 เป็นโมเดล open-source ตระกูลล่าสุดจาก Google DeepMind ที่เน้นความสามารถด้าน Multimodal และการใช้เหตุผล (Reasoning) ขั้นสูง

## Key Model Variants
1. **Edge Models (E2B / E4B):** ออกแบบมาเพื่อรันบน Mobile/Laptops (Context 128K)
2. **Workstation Models:**
   - **26B A4B (MoE):** ใช้ 8 active experts จาก 128 experts (Active จริงเพียง 3.8B) รันเร็วและฉลาด (Context 256K)
   - **31B (Dense):** รุ่นมาตรฐานสำหรับงานที่ต้องการ Frontier Intelligence (Context 256K)

## Advanced Capabilities
- **Reasoning:** รองรับ "Thinking Mode" โดยกำเนิด (native system prompt support)
- **Multimodality:**
  - รองรับ Text + Image (ทุกรุ่น)
  - รุ่นเล็ก (E2B/E4B) รองรับ Audio ด้วย
- **Variable Image Resolution:** สามารถปรับ Visual Token Budget (70 - 1120 tokens) ตามความละเอียดของภาพที่ต้องการประมวลผล (เช่น OCR ใช้สูง, Captioning ใช้ต่ำ)

## Implementation Notes (พี่เอิบ)
- **Ollama Command:** `ollama run gemma4:26b` (แนะนำรุ่น MoE สำหรับความเร็วบน VPS)
- **System Prompt:** ต้องระบุความต้องการในการ "Think" ที่ต้นของ System Prompt เพื่อเปิดใช้งานโหมดเหตุผล
- **KV Cache Optimization:** แนะนำให้ลองใช้ร่วมกับ [[turboquant]] เพื่อรับมือกับ Context 256K บนสเปก KVM8

---
*ความเกี่ยวข้องกับทีม:* โปรเจกต์ ReNeural ใช้ [[team-structure|ReNeural Agent]] ในการรันโมเดลนี้
*Last Updated: 2026-04-13 by อัญญา (Anya)*
-e 
**เอกสารดิบในระบบ:** [[../raw/gemma4-source|Gemma 4 Source Data]]

### แหล่งอ้างอิง (Sources)
- [Ollama Library: Gemma 4](https://ollama.com/library/gemma4)
- [Tim Carambat YouTube: TurboQuant](https://youtu.be/GY7q9ZqM8bw)
