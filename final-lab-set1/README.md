# Final Lab – Task Management System

โปรเจกต์นี้เป็นส่วนหนึ่งของรายวิชา ENGSE207  
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

## Project Structure
final-lab-set1
│
├── auth-service
├── task-service
├── log-service
├── frontend
├── db
├── nginx
├── screenshots
├── scripts
├── docker-compose.yml
└── README.md

---

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
