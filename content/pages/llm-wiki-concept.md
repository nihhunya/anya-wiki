---
type: concept
tags: [AI, KnowledgeManagement]
related: [Andrej-Karpathy, Self-Evolving-Memory]
---

# 🧠 LLM Wiki Concept (by Andrej Karpathy)

## 🌟 Definition
ระบบจัดการความรู้ส่วนตัว (Personal Knowledge Base) ที่ใช้ LLM เป็น "Compiler" หรือ "รวบรวมและเรียบเรียง" ข้อมูลดิบให้กลายเป็นฐานข้อมูล Markdown ที่เชื่อมโยงกันอย่างต่อเนื่อง (Compounding) แทนการใช้ระบบค้นหาชั่วคราว (RAG) แบบเดิมๆ

## 🏛️ Architecture (3 Layers)
1. **Raw Sources:** ข้อมูลต้นฉบับที่คงที่ (Immutable) เช่น PDF, คลิปวิดีโอ, แชท, เว็บไซต์ (Markdown) ใน `/wiki/raw/`
2. **The Wiki:** ไฟล์ Markdown ใน `/wiki/pages/` ที่ AI สร้างและดูแลทั้งหมด (Summaries, Entity Pages, Concepts)
3. **The Schema/Execution:** กฎการทำงานที่ระบุใน `CLAUDE.md` หรือ `SOUL.md` (สำหรับอัญญา) และการรัน AI Engine (Claude Code/OpenClaw) เพื่อจัดการฐานข้อมูล

## 🛠️ Core Operations (Workflow)
*อ้างอิงจาก [[karpathy-llm-wiki-guide]] (คลิป iXd0t60YmMw)*
- **Ingest:** การนำข้อมูลใหม่จาก `raw/` มาย่อย และอัปเดตกระจายไปยังหน้า Wiki ใน `pages/` ที่เกี่ยวข้อง (One-to-many update) โดยใช้ Obsidian Web Clipper หรือเครื่องมืออื่น
- **Query:** การถามคำถามโดยอ้างอิงจาก Wiki และเขียนคำตอบกลับเข้าไปเป็นหน้าใหม่เพื่อสะสมความรู้ (Compounding)
- **Lint:** การตรวจสุขภาพ Wiki (หาลิ้งค์เสีย, ข้อมูลขัดแย้ง, ข้อมูลที่ล้าสมัย, จัดระเบียบ Schema)

## 🛠️ Publishing (Knowledge Sharing)
*อ้างอิงจาก [[quartz-obsidian-website]] (คลิป zGFroBGud7w)*
- หากต้องการทำวิกิส่วนตัวให้กลายเป็นเว็บไซต์สาธารณะ สามารถใช้ **Quartz** (Static Site Generator) เพื่อแปลงไฟล์ Markdown ใน Obsidian ให้เป็นเว็บและโฮสต์บน GitHub Pages ได้ฟรี

## 💎 Key Benefits
- **Compounding Knowledge:** ยิ่งใช้นาน ระบบยิ่งฉลาดขึ้นและจดจำบริบทเจ้าของได้ลึกซึ้ง
- **Human-Readable:** มนุษย์อ่านและแก้ไขไฟล์ได้โดยตรงผ่าน Obsidian
- **Self-Healing:** ระบบตรวจสอบความถูกต้องของตัวเองสม่ำเสมอผ่านการ Linting

---
*Updated and Corrected by Anya (AI Secretary) on 2026-04-14*
