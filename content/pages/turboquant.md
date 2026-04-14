# TurboQuant (KV Cache Quantization)

## Overview
TurboQuant เป็นเทคโนโลยีการทำ Quantization รูปแบบใหม่จาก Google Research (เผยแพร่ช่วงเดือนมีนาคม-เมษายน 2026) ที่เน้นการบีบอัด **KV Cache** ให้เล็กลงอย่างมหาศาล (เหลือเพียง 3-bits) โดยไม่สูญเสียความแม่นยำ (Lossless-like performance)

## Key Technical Features
- **KV Cache Compression:** ลดการใช้ VRAM ในส่วนของ Context Window ลง 4-8 เท่า
- **Accuracy:** รักษาความแม่นยำได้ดีกว่าการทำ Quantization แบบเดิม (เช่น 4-bit/8-bit ทั่วไป)
- **Extreme Context:** ช่วยให้เครื่องคอมพิวเตอร์ทั่วไป (Consumer Hardware) รัน Model เดียวกันแต่รองรับ Context Window ที่ยาวขึ้นมากได้

## Implementation (Self-Hosted)
- **Tooling:** ปัจจุบันเริ่มมีการรวม (Merge) เข้ากับ `llama.cpp` และเครื่องมืออย่าง `Ollama`
- **Linux VPS (KVM8):** สามารถติดตั้งได้โดยการ Build llama.cpp จาก Source และใช้ Flags ใหม่สำหรับการทำ KV Cache Quantization
- **RAM Requirement:** แนะนำ 16GB+ สำหรับเสถียรภาพในการรัน Model ขนาดใหญ่ร่วมกับ TurboQuant

## Development Roadmap (พี่เอิบ)
- [ ] ทดลองติดตั้งบนเครื่องใหม่ (KVM8)
- [ ] ทดสอบประสิทธิภาพการประหยัด RAM เทียบกับ Standard Quantization สำหรับ [[gemma4]]
- [ ] จดบันทึกผลการลองผิดลองถูกในโปรเจกต์ [[team-structure|ReNeural]]

---
*Last Updated: 2026-04-13 by อัญญา (Anya)*
