# Final Lab – Task Management System

final-lab-set1 นี้เป็นส่วนหนึ่งของรายวิชา ENGSE207 Software Architecture
เป็นระบบจัดการงาน (Task Management System) ที่พัฒนาด้วยสถาปัตยกรรม Microservices โดยใช้ Node.js, Express และ PostgreSQL

---

## Features

- User Authentication (Login ด้วย JWT)
- Task Management (Create / Read / Update / Delete Task)
- Logging Service สำหรับบันทึกการใช้งานระบบ
- Role-based Access Control (Admin / Member)
- API Gateway ผ่าน Nginx
- Containerization ด้วย Docker

---

## System Architecture

ระบบถูกแบ่งออกเป็นหลาย service ดังนี้

- **auth-service** – จัดการการ login และ JWT authentication
- **task-service** – จัดการ Task CRUD
- **log-service** – บันทึก activity logs
- **frontend** – หน้าเว็บสำหรับใช้งานระบบ
- **db** – PostgreSQL database
- **nginx** – Reverse proxy สำหรับ API

---

## 🏗️ Architecture
 
[architecture.drawio.png](https://github.com/pakinkantawong/final-lab/blob/main/final-lab-set1/screenshots/architecture.drawio.png)


## Project Structure
```
final-lab-set1/
├── README.md
├── docker-compose.yml
├── .env.example
├── .gitignore
│
├── nginx/
│   ├── nginx.conf              # HTTPS + reverse proxy config
│   ├── Dockerfile
│   └── certs/
│       ├── cert.pem
│       └── key.pem
│
├── frontend/
│   ├── Dockerfile
│   ├── index.html              # Task Board UI (Login + CRUD Tasks + JWT inspector)
│   └── logs.html               # Log Dashboard (ดึงจาก /api/logs)
│
├── auth-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       ├── index.js
│       ├── routes/auth.js
│       ├── middleware/jwtUtils.js
│       └── db/db.js
│
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
│
├── log-service/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
│       └── index.js
│
├── db/
│   └── init.sql                # Schema + Seed Users ทั้งหมด
│
├── scripts/
│   └── gen-certs.sh            # สร้าง self-signed cert
│
└── screenshots/
    ├── 01_docker_running.png
    ├── 02_https_browser.png
    ├── 03_login_success.png
    ├── 04_login_fail.png
    ├── 05_create_task.png
    ├── 06_get_tasks.png
    ├── 07_update_task.png
    ├── 08_delete_task.png
    ├── 09_no_jwt_401.png
    ├── 10_logs_api.png
    ├── 11_rate_limit.png
    └── 12_frontend_screenshot.png
```
 
 

## Requirements

ก่อนรันโปรเจกต์ต้องติดตั้ง

- Docker
- Docker Compose
- Node.js
- Git

---

## Installation

1. Clone repository
git clone https://github.com/pakinkantawong/final-lab.git

cd final-lab/final-lab-set1


2. ตั้งค่า environment


cp .env.example .env


3. Run docker
docker compose up -d --build
---
