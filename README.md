# N8N Automation Platform 🤖

N8N เป็นแพลตฟอร์มการทำงงานอัตโนมัติแบบโอเพนซอร์สที่ช่วยให้คุณสร้าง workflow และเชื่อมต่อระบบต่างๆ ได้อย่างง่ายดาย

## 🚀 Quick Start

### ✨ การรันแบบพื้นฐาน (Basic Setup)

สำหรับการใช้งานในเครื่องหรือเซิร์ฟเวอร์ที่มี IP Address คงที่

```bash
docker compose -p n8n-starter-basic -f docker-compose-basic.yml up -d
```

เข้าใช้งานได้ที่: `http://localhost:5678`

### 🌐 การรันแบบมี Ngrok (Public Access)

สำหรับการใช้งานที่ต้องเข้าถึงจากภายนอกหรือต้องรับ Webhook จากบริการต่างๆ

```bash
docker compose -p n8n-starter-ngrok -f docker-compose-ngrok.yml up -d
```

Ngrok จะสร้าง Public URL ให้อัตโนมัติ เช็คได้ใน logs:
```bash
docker logs n8n-starter-ngrok-ngrok-1
```

## 🔄 การอัปเดต

### อัปเดต N8N เป็นเวอร์ชันล่าสุด

```bash
docker compose pull && docker compose up -d
```

### อัปเดตแบบเจาะจง (ถ้าใช้ compose file เฉพาะ)

```bash
# สำหรับ basic setup
docker compose -p n8n-starter-basic -f docker-compose-basic.yml pull
docker compose -p n8n-starter-basic -f docker-compose-basic.yml up -d

# สำหรับ ngrok setup
docker compose -p n8n-starter-ngrok -f docker-compose-ngrok.yml pull
docker compose -p n8n-starter-ngrok -f docker-compose-ngrok.yml up -d
```

## 🛠️ การจัดการระบบ

### ดูสถานะ Container

```bash
# สำหรับ basic setup
docker compose -p n8n-starter-basic ps

# สำหรับ ngrok setup
docker compose -p n8n-starter-ngrok ps
```

### ดู Logs

```bash
# สำหรับ basic setup
docker compose -p n8n-starter-basic logs -f

# สำหรับ ngrok setup
docker compose -p n8n-starter-ngrok logs -f n8n
docker compose -p n8n-starter-ngrok logs -f ngrok
```

### หยุดและเริ่มใหม่

```bash
# หยุดระบบ
docker compose -p n8n-starter-basic down
# หรือ
docker compose -p n8n-starter-ngrok down

# เริ่มใหม่
docker compose -p n8n-starter-basic up -d
# หรือ
docker compose -p n8n-starter-ngrok up -d
```

## 📁 โครงสร้าง Files

```
├── docker-compose-basic.yml    # Configuration สำหรับการใช้งานพื้นฐาน
├── docker-compose-ngrok.yml    # Configuration สำหรับการใช้งานกับ Ngrok
├── .env                        # Environment variables (optional)
├── workflows/                  # ตัวอย่าง Workflows สำหรับ Import
│   ├── basic-automation/       # Workflow พื้นฐาน
│   │   ├── email-notification.json
│   │   ├── data-backup.json
│   │   └── schedule-tasks.json
│   ├── social-media/          # Social Media Automation
│   │   ├── auto-post-facebook.json
│   │   ├── instagram-scheduler.json
│   │   └── twitter-engagement.json
│   ├── business-process/      # กระบวนการทางธุรกิจ
│   │   ├── invoice-processing.json
│   │   ├── customer-onboarding.json
│   │   └── inventory-management.json
│   ├── data-integration/      # การเชื่อมต่อข้อมูล
│   │   ├── database-sync.json
│   │   ├── api-integration.json
│   │   └── file-processing.json
│   └── monitoring/            # Monitoring และ Alerting
│       ├── server-health-check.json
│       ├── website-monitor.json
│       └── error-notification.json
└── README.md                   # Documentation นี้
```

## ⚙️ Configuration

### Environment Variables

สร้างไฟล์ `.env` เพื่อกำหนด configuration:

```env
# Database (ถ้าใช้ PostgreSQL)
POSTGRES_DB=n8n
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your_db_password

# Ngrok (ถ้าใช้ ngrok setup)
NGROK_AUTHTOKEN=
NGROK_DOMAIN=
```

## 🔧 Troubleshooting

### ปัญหาที่พบบ่อย

**1. Port 5678 ถูกใช้งานแล้ว**
```bash
# เช็ค process ที่ใช้ port
lsof -i :5678
# หยุด process หรือเปลี่ยน port ใน docker-compose
```

**2. Container ไม่สามารถเริ่มได้**
```bash
# เช็ค logs
docker compose logs
# ลบ container และเริ่มใหม่
docker compose down -v
docker compose up -d
```

**3. Ngrok ไม่ทำงาน**
- ตรวจสอบ NGROK_AUTHTOKEN
- เช็คว่า ngrok account มี tunnel limit เพียงพอ

### การ Backup และ Restore

```bash
# Backup workflows และ credentials
docker exec n8n-container n8n export:workflow --backup --output=/backup/
docker exec n8n-container n8n export:credentials --backup --output=/backup/

# Copy ออกจาก container
docker cp n8n-container:/backup/ ./backup/
```

## 📚 เพิ่มเติม

- [N8N Official Documentation](https://docs.n8n.io/)
- [N8N Community](https://community.n8n.io/)
- [Workflow Templates](https://n8n.io/workflows/)

## 🐛 รายงานปัญหา

หากพบปัญหาการใช้งาน สามารถรายงานได้ที่:
- GitHub Issues
- N8N Community Forum

---

**Happy Automating! 🎉**