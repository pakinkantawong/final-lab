# ENGSE207 Software Architecture
## Final Lab Set 1: Microservices + HTTPS + Lightweight Logging

---

## 1. ข้อมูลรายวิชาและสมาชิก

**รายวิชา:** ENGSE207 Software Architecture
**ชื่องาน:** Final Lab — ชุดที่ 1: Microservices + HTTPS + Lightweight Logging

### สมาชิกในกลุ่ม

| ชื่อ-สกุล | รหัสนักศึกษา |
|---|---|
| นายภาคิน กันทะวงค์ | 6743210062-5 |
| นายธวัชชัย สุหงษา | 6743210019-5 |

**Repository:** `final-lab-set1/`

---

## 2. ภาพรวมของระบบ

Final Lab ชุดที่ 1 เป็นการพัฒนาระบบ Task Board แบบ Microservices โดยเน้นหัวข้อสำคัญดังนี้

- การทำงานแบบแยก service
- การใช้ Nginx เป็น API Gateway
- การเปิดใช้งาน HTTPS ด้วย Self-Signed Certificate
- การยืนยันตัวตนด้วย JWT
- การจัดเก็บ log แบบ Lightweight Logging ผ่าน Log Service
- การเชื่อมต่อ Frontend กับ Backend ผ่าน HTTPS

> **หมายเหตุ:** งานชุดนี้ไม่มี Register และใช้เฉพาะ Seed Users ที่กำหนดไว้ในฐานข้อมูล

---

## 3. วัตถุประสงค์ของงาน

งานนี้มีจุดมุ่งหมายเพื่อฝึกให้นักศึกษาสามารถ

- ออกแบบระบบแบบ Microservices ในระดับพื้นฐาน
- ใช้ Nginx เป็น reverse proxy และ TLS termination
- ใช้ JWT สำหรับ authentication ระหว่าง frontend และ backend
- ออกแบบ logging flow ผ่าน REST API และจัดเก็บ log ลงฐานข้อมูล
- ใช้ Docker Compose เพื่อรวมทุก service ให้ทำงานร่วมกันได้

---

## 4. Architecture Overview

```
Browser / Postman
       │
       │ HTTPS :443
       ▼
┌──────────────────────────────────────────┐
│           Nginx (API Gateway)            │
│   SSL Termination · Rate Limiting        │
└──────┬───────────┬──────────┬────────────┘
       │           │          │          │
  /api/auth  /api/tasks  /api/logs       /
       │           │          │     (frontend)
       ▼           ▼          ▼
  auth-service  task-service  log-service
       │           │          │
       └───────────┴──────────┘
                   │
                   ▼
          PostgreSQL (shared DB)
```

### Services ที่ใช้ในระบบ

| Service | หน้าที่ |
|---|---|
| `nginx` | API Gateway, HTTPS, rate limiting |
| `frontend` | หน้าเว็บ Task Board และ Log Dashboard |
| `auth-service` | Login, Verify, Me |
| `task-service` | CRUD Tasks |
| `log-service` | รับและแสดง logs |
| `postgres` | Shared database |

---

## 5. โครงสร้าง Repository

```
final-lab-set1/
├── README.md
├── TEAM_SPLIT.md
├── INDIVIDUAL_REPORT_6743210062-5.md
├── INDIVIDUAL_REPORT_6743210019-5.md
├── docker-compose.yml
├── .env.example
├── nginx/
│   ├── nginx.conf
│   ├── Dockerfile
│   └── certs/
│       ├── cert.pem
│       └── key.pem
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   └── logs.html
├── auth-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       ├── index.js
│       ├── routes/auth.js
│       ├── middleware/jwtUtils.js
│       └── db/db.js
├── task-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       ├── index.js
│       ├── routes/tasks.js
│       ├── middleware/
│       │   ├── authMiddleware.js
│       │   └── jwtUtils.js
│       └── db/db.js
├── log-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       └── index.js
├── db/
│   └── init.sql
├── scripts/
│   └── gen-certs.sh
└── screenshots/
    ├── 01_docker_running.png
    ├── 02_https_browser.png
    └── 03_login_success.png
    └── 04_login_fail.png
    └── 05_create_task.png
    └── 06_get_tasks.png
    └── 07_update_task.png
    └── 08_delete_task.png
    └── 09_no_jwt_401.png
    └── 10_logs_api.png
    └── 11_rate_limit.png
```

---

## 6. เทคโนโลยีที่ใช้

| Category | Technology |
|---|---|
| Runtime | Node.js / Express.js |
| Database | PostgreSQL |
| Reverse Proxy | Nginx |
| Containerization | Docker / Docker Compose |
| Frontend | HTML / CSS / JavaScript |
| Authentication | JWT |
| Password Hashing | bcryptjs |

---

## 7. การตั้งค่าและการรันระบบ

### 7.1 สร้าง Self-Signed Certificate

```bash
chmod +x scripts/gen-certs.sh
./scripts/gen-certs.sh
```

### 7.2 สร้างไฟล์ .env

คัดลอกจาก `.env.example` แล้วกำหนดค่าตามต้องการ

```bash
cp .env.example .env
```

ตัวอย่างค่าใน `.env`:

```env
POSTGRES_DB=taskboard
POSTGRES_USER=admin
POSTGRES_PASSWORD=secret123
JWT_SECRET=engse207-super-secret-change-me
JWT_EXPIRES=1h
```

### 7.3 สร้าง bcrypt hash สำหรับ Seed Users

> ⚠️ งานชุดนี้กำหนดให้สร้าง bcrypt hash เองก่อนรันระบบ

รันคำสั่งต่อไปนี้เพื่อสร้าง hash:

```bash
node -e "const b=require('bcryptjs'); console.log(b.hashSync('alice123',10))"
node -e "const b=require('bcryptjs'); console.log(b.hashSync('bob456',10))"
node -e "const b=require('bcryptjs'); console.log(b.hashSync('adminpass',10))"
```

จากนั้นนำค่าที่ได้ไปแทนในไฟล์ `db/init.sql`

### 7.4 รันระบบ

```bash
docker compose down -v
docker compose up --build
```

### 7.5 เปิดใช้งานผ่าน Browser

| หน้า | URL |
|---|---|
| Frontend (Task Board) | https://localhost |
| Log Dashboard | https://localhost/logs.html |

> **หมายเหตุ:** เนื่องจากใช้ self-signed certificate browser อาจขึ้นคำเตือนด้านความปลอดภัย ให้คลิก **Advanced** → **Proceed to localhost** เพื่อเข้าทดสอบ

---

## 8. Seed Users สำหรับทดสอบ

| Username | Email | Password | Role |
|---|---|---|---|
| alice | alice@lab.local | alice123 | member |
| bob | bob@lab.local | bob456 | member |
| admin | admin@lab.local | adminpass | admin |

> **หมายเหตุ:** ต้อง generate bcrypt hash จริงแล้วแทนค่าลงใน `db/init.sql` ก่อน login (ดูขั้นตอนที่ 7.3)

---

## 9. API Summary

### 🔐 Auth Service — `/api/auth`

| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| POST | `/api/auth/login` | เข้าสู่ระบบ รับ JWT Token | ❌ |
| GET | `/api/auth/verify` | ตรวจสอบความถูกต้องของ Token | ✅ |
| GET | `/api/auth/me` | ดูข้อมูล user ปัจจุบัน | ✅ |
| GET | `/api/auth/health` | Health check | ❌ |

### ✅ Task Service — `/api/tasks`

| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| GET | `/api/tasks/health` | Health check | ❌ |
| GET | `/api/tasks/` | ดึง Task ทั้งหมด | ✅ |
| POST | `/api/tasks/` | สร้าง Task ใหม่ | ✅ |
| PUT | `/api/tasks/:id` | แก้ไข Task | ✅ |
| DELETE | `/api/tasks/:id` | ลบ Task | ✅ |

### 📋 Log Service — `/api/logs`

| Method | Endpoint | Description | Auth Required |
|---|---|---|:---:|
| POST | `/api/logs/internal` | บันทึก log (internal use) | ❌ |
| GET | `/api/logs/` | ดึง log ทั้งหมด | ✅ |
| GET | `/api/logs/stats` | สถิติ log | ✅ |
| GET | `/api/logs/health` | Health check | ❌ |

---

## 10. การทดสอบระบบ

### ลำดับการทดสอบ

1. รัน `docker compose up --build`
2. เปิด `https://localhost`
3. Login ด้วย seed users
4. สร้าง task ใหม่
5. ดูรายการ task
6. แก้ไข task
7. ลบ task
8. ทดสอบกรณีไม่มี JWT → ต้องได้ `401 Unauthorized`
9. ทดสอบ Log Dashboard
10. ทดสอบ rate limiting ของ login

### ตัวอย่าง curl

```bash
BASE="https://localhost"

# Login และรับ Token
TOKEN=$(curl -sk -X POST $BASE/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"alice@lab.local","password":"alice123"}' | \
  python3 -c "import sys,json; print(json.load(sys.stdin)['token'])")

# ดึงรายการ Task
curl -sk $BASE/api/tasks/ -H "Authorization: Bearer $TOKEN"

# สร้าง Task ใหม่
curl -sk -X POST $BASE/api/tasks/ \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"title":"My Task","description":"Task details"}'

# ทดสอบไม่มี JWT → 401
curl -sk $BASE/api/tasks/
```

---

## 11. Screenshots ที่แนบในงาน

| # | ไฟล์ | คำอธิบาย |
|---|---|---|
| 01 | `01_docker_running.png` | Docker containers ทั้งหมดรันสำเร็จ |
| 02 | `02_https_browser.png` | เปิดเว็บผ่าน HTTPS |
| 03 | `03_login_success.png` | Login สำเร็จ — ได้รับ JWT Token |
| 04 | `04_login_fail.png` | Login ล้มเหลว — ข้อมูลไม่ถูกต้อง |
| 05 | `05_create_task.png` | สร้าง Task ใหม่สำเร็จ |
| 06 | `06_get_tasks.png` | ดึงรายการ Task ทั้งหมด |
| 07 | `07_update_task.png` | แก้ไข Task สำเร็จ |
| 08 | `08_delete_task.png` | ลบ Task สำเร็จ |
| 09 | `09_no_jwt_401.png` | ไม่มี JWT Token → 401 Unauthorized |
| 10 | `10_logs_api.png` | ดู Logs ผ่าน API |
| 11 | `11_rate_limit.png` | Rate Limit ทำงาน |
| 12 | `12_frontend_screenshot.png` | หน้า Frontend ภาพรวม |

---

## 12. การแบ่งงานของทีม

รายละเอียดการแบ่งงานของสมาชิกอยู่ในไฟล์:

📄 [`TEAM_SPLIT.md`](./TEAM_SPLIT.md)

และรายงานรายบุคคลของสมาชิกแต่ละคนอยู่ในไฟล์:

📄 [`INDIVIDUAL_REPORT_6743210062-5.md`](./INDIVIDUAL_REPORT_6743210062-5.md) — นายภาคิน กันทะวงค์
📄 [`INDIVIDUAL_REPORT_6743210019-5.md`](./INDIVIDUAL_REPORT_6743210019-5.md) — นายธวัชชัย สุหงษา

---

## 13. ปัญหาที่พบและแนวทางแก้ไข

| # | ปัญหา | สาเหตุ | วิธีแก้ไข |
|---|---|---|---|
| 1 | Seed users login ไม่ได้ | Password ใน `init.sql` เป็น plain text แต่ระบบใช้ bcrypt | Generate bcrypt hash จริงแล้วแทนค่าใน `init.sql` |
| 2 | JWT verify ไม่ผ่านระหว่าง services | Auth และ Task Service ใช้ `JWT_SECRET` คนละค่า | กำหนด `JWT_SECRET` ใน `.env` และส่งผ่าน `docker-compose.yml` |
| 3 | Container start ก่อน PostgreSQL พร้อม | Service connect DB ก่อน DB พร้อม | เพิ่ม `depends_on` และ retry logic ใน `db/db.js` |
| 4 | Docker volume เก็บข้อมูลเดิม | Volume ยังคงข้อมูล seed เก่า ทำให้ seed ใหม่ไม่ทำงาน | รัน `docker compose down -v` ก่อน rebuild |
| 5 | Browser แจ้งเตือน SSL certificate | ใช้ self-signed certificate | คลิก Advanced → Proceed to localhost |

---

## 14. ข้อจำกัดของระบบ

- ใช้ self-signed certificate สำหรับการพัฒนา **ไม่เหมาะสำหรับ production จริง**
- ใช้ shared database เพียง 1 ก้อนสำหรับทุก service
- ยังไม่มีระบบ register (ใช้ได้เฉพาะ seed users)
- Logging เป็นแบบ lightweight ไม่ใช่ centralized observability platform เต็มรูปแบบ
- เหมาะสำหรับการเรียนรู้ architecture ระดับพื้นฐานและการต่อยอดไป Set 2

---

## 15. การต่อยอดไปยัง Set 2

งาน Set 1 เป็นพื้นฐานสำคัญสำหรับ Set 2 โดยประเด็นที่จะต่อยอด ได้แก่

- เพิ่ม Register API
- เพิ่ม User Service แยกออกมาต่างหาก
- เปลี่ยนจาก shared DB ไปเป็น database-per-service
- Deploy บน Railway Cloud
- ออกแบบ gateway strategy สำหรับหลาย service

---

## 16. ภาคผนวก

### ไฟล์สำคัญใน Repository

| ไฟล์ | คำอธิบาย |
|---|---|
| `docker-compose.yml` | Orchestration ทุก container |
| `nginx/nginx.conf` | Reverse proxy และ SSL config |
| `db/init.sql` | Schema และ seed users |
| `auth-service/src/routes/auth.js` | Login, verify, me endpoints |
| `task-service/src/routes/tasks.js` | CRUD task endpoints |
| `log-service/src/index.js` | Log collection และ expose |
| `frontend/index.html` | Task Board UI |
| `frontend/logs.html` | Log Dashboard UI |
