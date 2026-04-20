# AgingOne Project - Raw Notes

**วันที่บันทึก:** 2026-04-20  
**ผู้บันทึก:** อัญญา

---

## ข้อมูลพื้นฐาน

- ชื่อโปรเจกต์: AgingOne
- ชื่อเอเจนต์: วัน (รุ่น พ.ศ. 2544)
- แพลตฟอร์ม: LINE
- Path: `/data/.openclaw/workspace-agingone/`

## ฟีเจอร์

### Health Consultation
- ให้คำแนะนำเรื่องยา
- แจ้งเวลาทานยา
- คำแนะนำสุขภาพเบื้องต้น

### Nutrition Advice
- เมนูอาหารผู้สูงอายุ
- อาหารที่ควรหลีกเลี่ยง
- สูตรอาหารง่ายๆ

### Memory System
- จำชื่อลูกค้าได้
- บันทึกประวัติแต่ละคน
- แยกโฟลเดอร์ลูกค้า

## โครงสร้างระบบ

```
agingone/
├── customers/
│   └── [customer_id]/
│       ├── profile.md
│       ├── history/
│       └── preferences.md
├── subagents/
│   ├── eldercare/
│   └── nutrition/
└── system/
    ├── scheduler/
    └── notifications/
```

## ความปลอดภัย

- Sanitization ทุกคำสั่ง
- Isolated sandbox
- Access control โดยพี่เอิบ
- No cross-talk ระหว่างลูกค้า

## TODO

- [ ] เพิ่มจำนวน Subagents
- [ ] ปรับปรุง UI/UX
- [ ] เพิ่มการแจ้งเตือน
- [ ] สร้าง dashboard

---

*Raw notes - Immutable source*
