# n8n Workflow Setup Guide

คู่มือการสร้างและจัดการ n8n workflows สำหรับระบบ inputbox

> **บันทึก:** คู่มือนี้มีไว้สำหรับผู้ต้องการสร้าง workflow ผ่าน n8n API  
> **อัปเดตล่าสุด:** 2026-04-20 by อัญญา 🌸

---

## 📋 สารบัญ

1. [ข้อมูลการเชื่อมต่อ](#ข้อมูลการเชื่อมต่อ)
2. [โครงสร้าง workflow พื้นฐาน](#โครงสร้าง-workflow-พื้นฐาน)
3. [วิธีสร้าง workflow](#วิธีสร้าง-workflow)
4. [ตัวอย่าง workflow สำหรับ inputbox](#ตัวอย่าง-workflow-สำหรับ-inputbox)
5. [Webhook URL](#webhook-url)

---

## ข้อมูลการเชื่อมต่อ

### Base URL
```
https://n8n.powpoy.com/api/v1
```

### API Key
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI2ZTA5ZjM0OS05Yzc1LTRlMDItODRkNy00NmZlMjQ0NjMyODAiLCJpc3MiOiJuOG4iLCJhdWQiOiJwdWJsaWMtYXBpIiwianRpIjoiMDZlNGZhMzAtZDQzZS00OGZmLTkzMjctMDA0ZjNmM2M5MmUxIiwiaWF0IjoxNzc2NjUyMDUyfQ.wJ16mzF5J9DkIczopwbXe4z6fvdjFmCC4UszITNJhkk
```

### HTTP Header
```
X-N8N-API-KEY: <API_KEY>
```

---

## โครงสร้าง workflow พื้นฐาน

### Node ที่จำเป็นสำหรับ inputbox:

1. **Webhook** (Trigger)
   - HTTP Method: POST
   - Path: `inputbox/trigger`
   - Response Mode: lastNode

2. **Code** (Classify & Route)
   - ใช้ JavaScript สำหรับแบ่งประเภทไฟล์
   - ตรวจสอบ label ของไฟล์

3. **Files** (Move File)
   - ย้ายไฟล์ไปยังโฟลเดอร์เป้าหมาย
   - Source Path: `={{ $json.path }}`
   - Destination Path: `={{ $json.destination }}`

---

## วิธีสร้าง workflow

### 1. เตรียมไฟล์ JSON

สร้างไฟล์ JSON ที่มีโครงสร้างดังนี้:

```json
{
  "name": "ชื่อ workflow",
  "nodes": [
    {
      "name": "Webhook",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [0, 0],
      "parameters": {
        "httpMethod": "POST",
        "path": "inputbox/trigger",
        "responseMode": "lastNode"
      }
    },
    {
      "name": "Classify & Route",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [200, 0],
      "parameters": {
        "jsCode": "// JavaScript logic here"
      }
    },
    {
      "name": "Move File",
      "type": "n8n-nodes-base.files",
      "typeVersion": 1,
      "position": [400, 0],
      "parameters": {
        "operation": "move",
        "sourcePath": "={{ $json.path }}",
        "destinationPath": "={{ $json.destination }}"
      }
    }
  ],
  "connections": {
    "Webhook": {
      "main": [[{"node": "Classify & Route", "type": "main", "index": 0}]]
    },
    "Classify & Route": {
      "main": [[{"node": "Move File", "type": "main", "index": 0}]]
    }
  },
  "settings": {
    "executionOrder": "v1"
  }
}
```

⚠️ **ห้ามใส่ field `active`** เนื่องจากเป็น read-only field

---

### 2. ส่ง API Request

ใช้ `curl` เพื่อส่งไฟล์ JSON:

```bash
curl -s -X POST "https://n8n.powpoy.com/api/v1/workflows" \
  -H "X-N8N-API-KEY: <API_KEY>" \
  -H "Content-Type: application/json" \
  -d @workflow.json
```

---

### 3. ตรวจสอบผลลัพธ์

API จะตอบมาเป็น JSON:

```json
{
  "id": "pkm3XvZyyCET4tlD",
  "name": "Process Input Box Files",
  "active": false,
  "webhookId": "4631c30a-95ec-444d-9d49-30c91316ebf8"
}
```

บันทึก `id` ไว้เพื่อใช้งานต่อไป

---

### 4. เปิดใช้งาน Workflow

**⚠️ ห้ามใช้ REST API เพื่อเปิด workflow!**

ให้เปิดใช้งานผ่าน **n8n Web UI**:

1. เข้า https://n8n.powpoy.com
2. Login ด้วย:
   - Username: admin
   - Password: `JBT5cFPH4Sz36ry`
3. หา workflow ที่สร้าง
4. ดับเบิลคลิกเปิด
5. กดปุ่ม **Activate** (toggle มุมขวาบน)

---

## ตัวอย่าง workflow สำหรับ inputbox

### Code Node (JavaScript)

```javascript
const items = $input.all();

for (const item of items) {
  const { file, label, path } = item.json;
  
  // แบ่งตาม label
  if (label.includes('expense') || label.includes('receipt')) {
    item.json.action = 'move_to_finance';
    item.json.destination = '/home/node/.openclaw/workspace/finance/receipts/';
  } else if (label.includes('travel') || label.includes('ค่าเดินทาง')) {
    item.json.action = 'move_to_travel';
    item.json.destination = '/home/node/.openclaw/workspace/finance/receipts/travel/';
  } else {
    item.json.action = 'archive';
    item.json.destination = '/home/node/.openclaw/workspace/inputbox/archived/';
  }
}

return items;
```

---

## Webhook URL

เมื่อเปิด workflow แล้ว Webhook URL จะเป็น:

```
https://n8n.powpoy.com/webhook/inputbox/trigger
```

หรืออาจมี random ID ต่อท้าย:

```
https://n8n.powpoy.com/webhook/inputbox/trigger-4a2b3c
```

---

## ✅ Checklist การสร้าง n8n workflow

- [ ] เตรียมไฟล์ JSON workflow
- [ ] ตรวจสอบว่าไม่มี field `active`
- [ ] ส่ง POST request ไปยัง API
- [ ] บันทึก workflow `id` จาก response
- [ ] เปิดใช้งานผ่าน Web UI
- [ ] ทดสอบส่ง webhook

---

## 🔗 หน้าที่เกี่ยวข้อง

- [[n8n-Installation-Guide]] - คู่มือการติดตั้ง n8n
- [[n8n-Integration-Strategy]] - กลยุทธ์การเชื่อมต่อ n8n

---

*บันทึก: 2026-04-20 by อัญญา 🌸*
