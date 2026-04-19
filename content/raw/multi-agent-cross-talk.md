# Multi-Agent Cross-Talk: ปัญหาและแนวทางแก้ไข

> บันทึกจากการ debug ระบบ OpenClaw v2026.4.15  
> กรณีศึกษา: AgingOne (วันนา) และ Main Agent (อัญญา)

---

## 1. Cross-Talk คืออะไร

Cross-Talk ในระบบ Multi-Agent คือปรากฏการณ์ที่ผู้ใช้ส่งข้อความไปยัง Agent A แต่ได้รับคำตอบจาก Agent B แทน ส่งผลให้ตัวตน (Identity) ที่แสดงออกมาไม่ตรงกับ Agent ที่ผู้ใช้ตั้งใจจะคุยด้วย

ในกรณีนี้: ผู้ใช้ทักผ่าน LINE OA ของวันนา → แต่อัญญา (main agent) ตอบกลับแทน

---

## 2. สาเหตุของปัญหา (Root Causes)

### 2.1 ขาด `bindings` ใน config

**ปัญหาหลักที่สำคัญที่สุด**

OpenClaw ใช้ `bindings` เพื่อ route ข้อความขาเข้าไปยัง agent ที่ถูกต้อง หากไม่มี bindings ระบบจะ fallback ไปใช้ `main` agent เสมอ

```json
// ❌ ไม่มี bindings — ข้อความทุกอันไปที่ main
{
  "channels": { "line": { ... } }
}

// ✅ มี bindings — route ถูกต้อง
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

### 2.2 มี Root-Level Token ใน `channels.line`

เมื่อมี `channelAccessToken` และ `channelSecret` ที่ root level ของ `channels.line` OpenClaw จะสร้าง default LINE handler ที่ผูกกับ main agent ก่อนที่จะ check sub-accounts

```json
// ❌ Root token ทำให้ main agent รับข้อความก่อน
"channels": {
  "line": {
    "channelAccessToken": "TOKEN_ของ_อัญญา",
    "channelSecret": "SECRET_ของ_อัญญา",
    "accounts": {
      "agingone": { ... }
    }
  }
}

// ✅ ย้าย token เข้าไปใน accounts แทน
"channels": {
  "line": {
    "enabled": true,
    "accounts": {
      "main": {
        "channelAccessToken": "TOKEN_ของ_อัญญา",
        "channelSecret": "SECRET_ของ_อัญญา",
        "webhookPath": "/line/main/webhook"
      },
      "agingone": {
        "channelAccessToken": "TOKEN_ของ_วันนา",
        "channelSecret": "SECRET_ของ_วันนา",
        "webhookPath": "/line/agingone/webhook"
      }
    }
  }
}
```

### 2.3 Model ID Format ไม่ถูกต้อง

OpenClaw v2026.4.15 ต้องการ model ID แบบ `provider/model` (มี prefix) แต่ config ใช้แบบไม่มี prefix ทำให้ระบบหา model ไม่เจอและ fallback ไป main agent

```json
// ❌ ไม่มี provider prefix
"model": "nvidia/nemotron-3-super-120b-a12b:free"

// ✅ มี provider prefix
"model": "openrouter/nvidia/nemotron-3-super-120b-a12b:free"
```

### 2.4 Agent-Level `models.json` ไม่มี Model ที่ระบุใน config

แต่ละ agent มีไฟล์ `models.json` แยกต่างหากใน path:
```
/home/node/.openclaw/agents/{agentId}/agent/models.json
```

หาก model ที่กำหนดใน `openclaw.json` ไม่มีอยู่ใน `models.json` ของ agent นั้น ระบบจะ fallback ไปใช้ model จาก main agent แทน

### 2.5 Path ผิดใน Workspace Files (SOUL.md)

```markdown
❌ Path ที่ใช้ใน SOUL.md (ผิด)
/data/.openclaw/workspace-agingone/customers/
/data/.npm-global/bin/openclaw

✅ Path จริงใน Container
/home/node/.openclaw/workspace-agingone/customers/
/usr/local/bin/openclaw
```

ผลที่ตามมา: bash command ที่วันนารันเพื่อเรียก subagent fail ทุกครั้ง ทำให้วันนาไม่มีคำตอบจะส่งให้ผู้ใช้

### 2.6 กฎขัดแย้งใน SOUL.md (Logic Conflict)

```markdown
กฎ A: "ถ้าไม่มี output จาก bash = ห้ามตอบ"
กฎ B: "ห้ามส่ง empty message เด็ดขาด ทุก turn ต้องมี text"
```

เมื่อ subagent ไม่ตอบ → กฎ A บอกห้ามตอบ แต่กฎ B บอกต้องตอบ → model วนลูปส่งข้อความซ้ำๆ จนกว่า session จะหมดเวลา

---

## 3. ลำดับการ Debug

```
ตรวจ Webhook URL ใน LINE Console
        ↓ ถูกต้อง
ตรวจ Log: docker logs openclaw
        ↓ พบ fallback ไป main
ตรวจ bindings ใน openclaw.json
        ↓ ไม่มี bindings
ตรวจ Root-Level Token
        ↓ พบ token ที่ root
ตรวจ Model Format
        ↓ ขาด provider prefix
ตรวจ agents/{id}/agent/models.json
        ↓ model ไม่ตรง
ตรวจ Path ใน SOUL.md
        ↓ path ผิดทั้งหมด
แก้ไขและทดสอบ ✅
```

---

## 4. Config ที่ถูกต้อง (openclaw.json)

โครงสร้างที่ถูกต้องสำหรับ Multi-Agent + Multi LINE OA:

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
    {
      "type": "route",
      "agentId": "main",
      "match": { "channel": "line", "accountId": "main" }
    },
    {
      "type": "route",
      "agentId": "agingone",
      "match": { "channel": "line", "accountId": "agingone" }
    }
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

## 5. Checklist ป้องกัน Cross-Talk

เมื่อเพิ่ม Agent ใหม่พร้อม LINE OA ทำตามนี้ทุกครั้ง:

- [ ] สร้าง LINE OA แยกต่างหาก และเก็บ Token + Secret
- [ ] เพิ่ม account ใน `channels.line.accounts` พร้อม `webhookPath` ที่ unique
- [ ] เพิ่ม binding ใน `bindings[]` ให้ครบ
- [ ] ใช้ model ID แบบ `openrouter/provider/model` (มี prefix เสมอ)
- [ ] ตั้ง Webhook URL ใน LINE Developer Console ให้ตรงกับ `webhookPath`
- [ ] สร้าง workspace folder และ SOUL.md ให้ agent ใหม่
- [ ] ตรวจ path ใน SOUL.md ว่าตรงกับ path จริงใน container
- [ ] ทดสอบโดยส่งข้อความจริงและดู log ยืนยันว่า agent ถูกตัวตอบ

---

## 6. Path Reference สำหรับ OpenClaw v2026.4.15

| สิ่งที่ต้องการ | Path จริงใน Container |
|---|---|
| openclaw binary | `/usr/local/bin/openclaw` |
| config หลัก | `/home/node/.openclaw/openclaw.json` |
| workspace main | `/home/node/.openclaw/workspace` |
| workspace agingone | `/home/node/.openclaw/workspace-agingone` |
| agent sessions | `/home/node/.openclaw/agents/{id}/sessions/` |
| agent models | `/home/node/.openclaw/agents/{id}/agent/models.json` |

---

## 7. สาเหตุที่ Config เก่า (v2026.4.5) ใช้งานได้ แต่ v2026.4.15 ไม่ได้

| ประเด็น | v2026.4.5 | v2026.4.15 |
|---|---|---|
| bindings | มี | ต้องเพิ่มเอง (ไม่มีใน template ใหม่) |
| model prefix | `openrouter/google/model` | เดิม แต่ต้องครบ |
| root token | ยังรองรับ | ทำให้ main รับแทน |
| path binary | `/data/.npm-global/bin/openclaw` | `/usr/local/bin/openclaw` |
| path workspace | `/data/.openclaw/` | `/home/node/.openclaw/` |

---

*อัปเดตล่าสุด: 19 เม.ย. 2569*
