# Multi-Agent Cross-Talk: ปัญหาและแนวทางแก้ไข

บันทึกจากกรณีศึกษาการ debug ระบบ OpenClaw v2026.4.15 เกี่ยวกับการทำงานร่วมกันระหว่างหลาย Agent และปัญหา Cross-Talk

> **แหล่งข้อมูลดิบ:** [`multi-agent-cross-talk.md`](../raw/multi-agent-cross-talk.md)
> **อัปเดตล่าสุด:** 19 เม.ย. 2569

---

## 🎯 ภาพรวม

**Cross-Talk คืออะไร?** ปรากฏการณ์ที่ผู้ใช้ส่งข้อความไปยัง Agent A แต่ได้รับคำตอบจาก Agent B แทน ส่งผลให้ตัวตน (Identity) ที่แสดงออกมาไม่ตรงกับ Agent ที่ผู้ใช้ตั้งใจจะคุยด้วย

**กรณีศึกษา:** ผู้ใช้ทักผ่าน LINE OA ของวันนา (AgingOne) → แต่ อัญญา (main agent) ตอบกลับแทน

---

## 🔍 สาเหตุของปัญหา (Root Causes)

### 1. ขาด `bindings` ใน config ⭐ ปัญหาหลัก
OpenClaw ใช้ `bindings` เพื่อ route ข้อความขาไปยัง Agent ที่ถูกต้อง

**❌ ไม่มี bindings:** ข้อความทุกอันไปที่ main agent
```json
{ "channels": { "line": { ... } } }
```

**✅ มี bindings:**
```json
{
  "bindings": [
    {
      "type": "route",
      "agentId": "agingone",
      "match": {
        "channel": "line",
        "accountId": "agingone"
      }
    }
  ]
}
```

### 2. Root-Level Token ใน `channels.line`
เมื่อมี `channelAccessToken` และ `channelSecret` ที่ root level จะทำให้ main agent รับข้อความก่อน sub-accounts

**✅ ควรย้าย token เข้าไปใน accounts** แทนที่จะวางไว้ root level

### 3. Model ID Format ไม่ถูกต้อง
OpenClaw v2026.4.15 ต้องการ model ID แบบ `provider/model` (มี prefix)

- ❌ `"model": "nvidia/nemotron-3-super-120b-a12b:free"`
- ✅ `"model": "openrouter/nvidia/nemotron-3-super-120b-a12b:free"`

### 4. Agent-Level `models.json` ขาด Model
แต่ละ agent มีไฟล์ `models.json` ใน:
```
/home/node/.openclaw/agents/{agentId}/agent/models.json
```

### 5. Path ผิดใน Workspace Files
- ❌ `/data/.openclaw/workspace-agingone/`
- ✅ `/home/node/.openclaw/workspace-agingone/`

### 6. กฎขัดแย้งใน SOUL.md
- กฎ A: "ถ้าไม่มี output จาก bash = ห้ามตอบ"
- กฎ B: "ห้ามส่ง empty message เด็ดขาด"

เมื่อ subagent ไม่ตอบ → model วนลูปส่งข้อความซ้ำๆ

---

## 🛠️ ลำดับการ Debug

1. ตรวจ Webhook URL ใน LINE Console
2. ตรวจ Log: `docker logs openclaw`
3. ตรวจ bindings ใน `openclaw.json`
4. ตรวจ Root-Level Token
5. ตรวจ Model Format
6. ตรวจ `agents/{id}/agent/models.json`
7. ตรวจ Path ใน SOUL.md

---

## ✅ Config ที่ถูกต้อง

### โครงสร้าง `openclaw.json` สำหรับ Multi-Agent + Multi LINE OA

```json
{
  "agents": {
    "defaults": {
      "model": "openrouter/nvidia/nemotron-3-super-120b-a12b:free"
    },
    "list": [
      {
        "id": "main",
        "model": "openrouter/nvidia/nemotron-3-super-120b-a12b:free",
        "workspace": "/home/node/.openclaw/workspace"
      },
      {
        "id": "agingone",
        "model": "openrouter/nvidia/nemotron-3-super-120b-a12b:free",
        "workspace": "/home/node/.openclaw/workspace-agingone"
      }
    ]
  },

  "bindings": [
    { "type": "route", "agentId": "main", "match": { "channel": "line", "accountId": "main" } },
    { "type": "route", "agentId": "agingone", "match": { "channel": "line", "accountId": "agingone" } }
  ],

  "channels": {
    "line": {
      "enabled": true,
      "accounts": {
        "main": {
          "channelAccessToken": "TOKEN_อัญญา",
          "channelSecret": "SECRET_อัญญา",
          "webhookPath": "/line/main/webhook"
        },
        "agingone": {
          "channelAccessToken": "TOKEN_วันนา",
          "channelSecret": "SECRET_วันนา",
          "webhookPath": "/line/agingone/webhook"
        }
      }
    }
  }
}
```

---

## 📋 Checklist ป้องกัน Cross-Talk

เมื่อเพิ่ม Agent ใหม่พร้อม LINE OA:

- [ ] สร้าง LINE OA แยกต่างหาก และเก็บ Token + Secret
- [ ] เพิ่ม account ใน `channels.line.accounts` พร้อม `webhookPath` ที่ unique
- [ ] เพิ่ม binding ใน `bindings[]` ให้ครบ
- [ ] ใช้ model ID แบบ `openrouter/provider/model` (มี prefix เสมอ)
- [ ] ตั้ง Webhook URL ใน LINE Developer Console ให้ตรงกับ `webhookPath`
- [ ] สร้าง workspace folder และ SOUL.md ให้ agent ใหม่
- [ ] ตรวจ path ใน SOUL.md ว่าตรงกับ path จริงใน container
- [ ] ทดสอบโดยส่งข้อความจริงและดู log ยืนยันว่า agent ถูกตัวตอบ

---

## 📂 Path Reference สำหรับ OpenClaw v2026.4.15

| สิ่งที่ต้องการ | Path จริงใน Container |
|---|---|
| openclaw binary | `/usr/local/bin/openclaw` |
| config หลัก | `/home/node/.openclaw/openclaw.json` |
| workspace main | `/home/node/.openclaw/workspace` |
| workspace agingone | `/home/node/.openclaw/workspace-agingone` |
| agent sessions | `/home/node/.openclaw/agents/{id}/sessions/` |
| agent models | `/home/node/.openclaw/agents/{id}/agent/models.json` |

---

## 🆚 ความแตกต่าง v2026.4.5 vs v2026.4.15

| ประเด็น | v2026.4.5 | v2026.4.15 |
|---|---|---|
| bindings | มี | ต้องเพิ่มเอง (ไม่มีใน template ใหม่) |
| model prefix | `openrouter/google/model` | เดิม แต่ต้องครบ |
| root token | ยังรองรับ | ทำให้ main รับแทน |
| path binary | `/data/.npm-global/bin/openclaw` | `/usr/local/bin/openclaw` |
| path workspace | `/data/.openclaw/` | `/home/node/.openclaw/` |

---

## 🔗 หน้าที่เกี่ยวข้อง

- [[Anya-Harness-Implementation]] - การประยุกต์ใช้ Harness Engineering
- [[LLM Wiki Protocol]] - ระเบียบวิธีในการจัดเก็บความรู้
- [[Session-Retrospective-2026-04-18]] - กรณีศึกษาความผิดพลาดขั้นรุนแรง

---
## 🔧 Micro-steps ที่เกี่ยวข้อง

- [Context Mix-up Fix](../micro-steps/context-mixup-fix.md) - คู่มือการแก้ไขการสลับตัวตนวันนา ↔ อัญญา
- [Cross-Talk Issue](../micro-steps/fix-cross-talk.md) - แนวทางการแก้ไขปัญหา Agent Identity Confusion

---

*สร้างตามข้อมูลจาก: [[multi-agent-cross-talk|Raw Source]]*
