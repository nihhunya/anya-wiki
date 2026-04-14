# 🕸️ Graphify Mapping System (Anya Implementation)

ระบบการสร้างแผนที่ความรู้แบบอัตโนมัติที่อัญญาใช้จัดการวิกิ เพื่อให้ Graph View ใน [[Publishing-Wiki|wiki.powpoy.com]] มีความสมบูรณ์และเชื่อมโยงกันอย่างมีระบบ

## 📋 ขั้นตอนการทำงาน (Standard Operating Procedure)

### Step 1: Metadata Enrichment (YAML Frontmatter)
ทุกไฟล์วิกิต้องมี Metadata อยู่ด้านบนสุดเพื่อระบุพิกัดในโครงข่าย:
- `type`: ชนิดของโหนด (concept, project, entity, source)
- `tags`: หมวดหมู่ใหญ่ (เช่น #AI, #Business, #Tech)
- `related`: รายชื่อหน้าที่เกี่ยวข้องสำหรับการทำ Auto-link

### Step 2: Semantic Hubbing
เมื่อพบเนื้อหาใหม่ อัญญาต้องมองหาหน้า "Hub" ที่จะใช้เป็นจุดรวมสาย:
- หากเป็นเรื่องเทคนิค -> เชื่อมไปที่ [[concepts]]
- หากเป็นเรื่องผลงาน -> เชื่อมไปที่ [[projects]]
- หากเป็นชื่อบุคคล/บริษัท -> เชื่อมไปที่ [[entities]]

### Step 3: Automated Cross-Linking
ระหว่างการทำ [[llm-wiki-concept|Ingest]] อัญญาต้องสแกนหา keyword ในวิกิปัจจุบัน (ใช้ grep/search) และใส่ `[[ ]]` ล้อมรอบคำเหล่านั้นในไฟล์ใหม่ทันที

### Step 4: Maintenance & Orphan Check (Linting)
รันการตรวจสุขภาพวิกิเพื่อหาหน้า "Orphan" (หน้าที่ไม่มีใคร Link หา) และทำการสร้าง Connection จากหน้า Hub ที่เกี่ยวข้องเข้าไปหาหน้าดังกล่าว

---
*บันทึกขั้นตอนโดย อัญญา เมื่อวันที่ 14 เมษายน 2026 เพื่อใช้เป็นคู่มือปฏิบัติงาน*
