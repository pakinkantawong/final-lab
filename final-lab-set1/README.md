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

-<?xml version="1.0" encoding="UTF-8"?>
<mxfile host="app.diagrams.net" modified="2024-01-01T00:00:00.000Z" agent="draw.io" version="21.0.0">
  <diagram name="final-lab-set1 Architecture" id="architecture">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1654" pageHeight="1169" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />

        <!-- ========== BROWSER ========== -->
        <mxCell id="browser" value="&lt;b&gt;Browser / Client&lt;/b&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontStyle=1;fontSize=13;arcSize=10;" vertex="1" parent="1">
          <mxGeometry x="577" y="40" width="220" height="56" as="geometry" />
        </mxCell>

        <!-- HTTPS label -->
        <mxCell id="label_https" value="HTTPS :443" style="text;html=1;align=center;verticalAlign=middle;resizable=0;points=[];autosize=1;strokeColor=none;fillColor=none;fontSize=11;fontColor=#1e4d78;" vertex="1" parent="1">
          <mxGeometry x="648" y="104" width="90" height="20" as="geometry" />
        </mxCell>

        <!-- Arrow: Browser -> Nginx -->
        <mxCell id="edge_browser_nginx" style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jettySize=auto;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;strokeColor=#1e4d78;strokeWidth=2;" edge="1" source="browser" target="nginx" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== DOCKER COMPOSE BOUNDARY ========== -->
        <mxCell id="docker_boundary" value="docker-compose.yml" style="points=[[0,0],[0.25,0],[0.5,0],[0.75,0],[1,0],[1,0.25],[1,0.5],[1,0.75],[1,1],[0.75,1],[0.5,1],[0.25,1],[0,1],[0,0.75],[0,0.5],[0,0.25]];shape=mxgraph.lean_mapping.electronic_info_flow_edge;whiteSpace=wrap;html=1;dashed=1;fillColor=none;strokeColor=#666666;fontSize=11;fontColor=#666666;verticalAlign=top;strokeWidth=1.5;spacingTop=4;" vertex="1" parent="1">
          <mxGeometry x="100" y="130" width="1174" height="660" as="geometry" />
        </mxCell>

        <!-- ========== NGINX ========== -->
        <mxCell id="nginx" value="&lt;b&gt;Nginx&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:11px&apos;&gt;Reverse Proxy · SSL Termination · Rate Limiting&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#0050ef;fontColor=#ffffff;strokeColor=#001DBC;fontStyle=1;fontSize=13;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="477" y="150" width="420" height="64" as="geometry" />
        </mxCell>

        <!-- Nginx certs box -->
        <mxCell id="nginx_certs" value="&lt;b&gt;nginx/certs/&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:10px&apos;&gt;cert.pem · key.pem&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontColor=#333333;fontSize=11;arcSize=10;" vertex="1" parent="1">
          <mxGeometry x="940" y="158" width="150" height="48" as="geometry" />
        </mxCell>
        <mxCell id="edge_nginx_certs" style="edgeStyle=orthogonalEdgeStyle;dashed=1;strokeColor=#999999;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1" source="nginx" target="nginx_certs" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- nginx.conf label -->
        <mxCell id="label_nginxconf" value="nginx/nginx.conf" style="text;html=1;align=center;verticalAlign=middle;fontSize=10;fontColor=#555555;strokeColor=none;fillColor=none;" vertex="1" parent="1">
          <mxGeometry x="940" y="210" width="150" height="20" as="geometry" />
        </mxCell>

        <!-- ========== ROUTING LABEL ROW ========== -->
        <mxCell id="label_auth_route" value="/api/auth" style="text;html=1;align=center;fontSize=11;fontColor=#005073;strokeColor=none;fillColor=none;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="190" y="248" width="100" height="20" as="geometry" />
        </mxCell>
        <mxCell id="label_task_route" value="/api/tasks" style="text;html=1;align=center;fontSize=11;fontColor=#4d1f91;strokeColor=none;fillColor=none;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="620" y="248" width="130" height="20" as="geometry" />
        </mxCell>
        <mxCell id="label_log_route" value="/api/logs" style="text;html=1;align=center;fontSize=11;fontColor=#7d4a00;strokeColor=none;fillColor=none;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="1040" y="248" width="100" height="20" as="geometry" />
        </mxCell>

        <!-- Arrow: Nginx -> Auth Service -->
        <mxCell id="edge_nginx_auth" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#006EAF;strokeWidth=2;exitX=0.2;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="nginx" target="auth_service" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- Arrow: Nginx -> Task Service -->
        <mxCell id="edge_nginx_task" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#6a0dad;strokeWidth=2;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="nginx" target="task_service" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- Arrow: Nginx -> Log Service -->
        <mxCell id="edge_nginx_log" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#b46504;strokeWidth=2;exitX=0.8;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="nginx" target="log_service" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== AUTH SERVICE ========== -->
        <mxCell id="auth_service" value="&lt;b&gt;Auth Service&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:11px&apos;&gt;Node.js · Express&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#006EAF;fontStyle=1;fontSize=13;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="120" y="290" width="220" height="60" as="geometry" />
        </mxCell>

        <!-- Auth internals -->
        <mxCell id="auth_internals" value="&lt;b&gt;src/&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size=10px&apos;&gt;routes/auth.js&lt;br&gt;middleware/jwtUtils.js&lt;br&gt;db/db.js&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#eff6ff;strokeColor=#006EAF;fontSize=11;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="120" y="370" width="220" height="72" as="geometry" />
        </mxCell>
        <mxCell id="edge_auth_src" style="edgeStyle=orthogonalEdgeStyle;strokeColor=#006EAF;strokeWidth=1;dashed=0;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="auth_service" target="auth_internals" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== TASK SERVICE ========== -->
        <mxCell id="task_service" value="&lt;b&gt;Task Service&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:11px&apos;&gt;Node.js · Express&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#e1d5e7;strokeColor=#6a0dad;fontStyle=1;fontSize=13;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="577" y="290" width="220" height="60" as="geometry" />
        </mxCell>

        <!-- Task internals -->
        <mxCell id="task_internals" value="&lt;b&gt;src/&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:10px&apos;&gt;routes/tasks.js&lt;br&gt;authMiddleware.js · jwtUtils.js&lt;br&gt;db/db.js&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f5f0ff;strokeColor=#6a0dad;fontSize=11;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="577" y="370" width="220" height="72" as="geometry" />
        </mxCell>
        <mxCell id="edge_task_src" style="edgeStyle=orthogonalEdgeStyle;strokeColor=#6a0dad;strokeWidth=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="task_service" target="task_internals" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== LOG SERVICE ========== -->
        <mxCell id="log_service" value="&lt;b&gt;Log Service&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:11px&apos;&gt;Node.js · Express&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffe6cc;strokeColor=#b46504;fontStyle=1;fontSize=13;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="1030" y="290" width="220" height="60" as="geometry" />
        </mxCell>

        <!-- Log internals -->
        <mxCell id="log_internals" value="&lt;b&gt;src/&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:10px&apos;&gt;index.js&lt;br&gt;Collect API activity logs&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff8f0;strokeColor=#b46504;fontSize=11;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="1030" y="370" width="220" height="72" as="geometry" />
        </mxCell>
        <mxCell id="edge_log_src" style="edgeStyle=orthogonalEdgeStyle;strokeColor=#b46504;strokeWidth=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="log_service" target="log_internals" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== JWT FLOW ARROW (Auth -> Task) ========== -->
        <mxCell id="edge_jwt_flow" value="JWT Token" style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jettySize=auto;strokeColor=#006400;strokeWidth=2;dashed=1;fontColor=#006400;fontStyle=3;fontSize=11;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1" source="auth_service" target="task_service" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="420" y="320" />
              <mxPoint x="420" y="270" />
              <mxPoint x="687" y="270" />
              <mxPoint x="687" y="320" />
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- ========== DB ARROWS ========== -->
        <mxCell id="edge_auth_db" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1e4d78;strokeWidth=1.5;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.2;entryY=0;entryDx=0;entryDy=0;" edge="1" source="auth_internals" target="postgres" parent="1">
          <mxCell as="geometry" />
        </mxCell>
        <mxCell id="edge_task_db" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1e4d78;strokeWidth=1.5;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="task_internals" target="postgres" parent="1">
          <mxCell as="geometry" />
        </mxCell>
        <mxCell id="edge_log_db" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#1e4d78;strokeWidth=1.5;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.8;entryY=0;entryDx=0;entryDy=0;" edge="1" source="log_internals" target="postgres" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== POSTGRESQL ========== -->
        <mxCell id="postgres" value="&lt;b&gt;PostgreSQL&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:11px&apos;&gt;db/init.sql · Schema + Seed Users&lt;/font&gt;" style="shape=cylinder3;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#1e4d78;fontStyle=1;fontSize=13;size=10;" vertex="1" parent="1">
          <mxGeometry x="527" y="530" width="320" height="80" as="geometry" />
        </mxCell>

        <!-- init.sql note -->
        <mxCell id="label_initsql" value="db/init.sql" style="text;html=1;align=center;fontSize=10;fontColor=#1e4d78;strokeColor=none;fillColor=none;fontStyle=2;" vertex="1" parent="1">
          <mxGeometry x="527" y="616" width="320" height="16" as="geometry" />
        </mxCell>

        <!-- ========== FRONTEND ========== -->
        <mxCell id="frontend_box" value="&lt;b&gt;Frontend (Nginx served)&lt;/b&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;fontStyle=1;fontSize=13;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="477" y="680" width="420" height="44" as="geometry" />
        </mxCell>

        <!-- Frontend files -->
        <mxCell id="frontend_index" value="&lt;b&gt;index.html&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:10px&apos;&gt;Task Board · Login · JWT Inspector&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff0f0;strokeColor=#b85450;fontSize=11;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="477" y="740" width="200" height="48" as="geometry" />
        </mxCell>
        <mxCell id="frontend_logs" value="&lt;b&gt;logs.html&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:10px&apos;&gt;Log Dashboard · /api/logs&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#fff0f0;strokeColor=#b85450;fontSize=11;arcSize=6;" vertex="1" parent="1">
          <mxGeometry x="697" y="740" width="200" height="48" as="geometry" />
        </mxCell>

        <mxCell id="edge_fe_index" style="edgeStyle=orthogonalEdgeStyle;strokeColor=#b85450;strokeWidth=1;exitX=0.3;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="frontend_box" target="frontend_index" parent="1">
          <mxCell as="geometry" />
        </mxCell>
        <mxCell id="edge_fe_logs" style="edgeStyle=orthogonalEdgeStyle;strokeColor=#b85450;strokeWidth=1;exitX=0.7;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="frontend_box" target="frontend_logs" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- Arrow: Postgres -> Frontend (data flow upward conceptually via services) -->
        <mxCell id="edge_db_fe" style="edgeStyle=orthogonalEdgeStyle;rounded=0;strokeColor=#cccccc;strokeWidth=1;dashed=1;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;" edge="1" source="postgres" target="frontend_box" parent="1">
          <mxCell as="geometry" />
        </mxCell>

        <!-- ========== SCRIPTS ========== -->
        <mxCell id="scripts_box" value="&lt;b&gt;scripts/gen-certs.sh&lt;/b&gt;&lt;br&gt;&lt;font style=&apos;font-size:10px&apos;&gt;สร้าง self-signed cert&lt;/font&gt;" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontColor=#333333;fontSize=11;arcSize=8;" vertex="1" parent="1">
          <mxGeometry x="120" y="700" width="200" height="48" as="geometry" />
        </mxCell>
        <mxCell id="edge_scripts_certs" style="edgeStyle=orthogonalEdgeStyle;dashed=1;strokeColor=#999999;strokeWidth=1;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;" edge="1" source="scripts_box" target="nginx_certs" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="360" y="724" />
              <mxPoint x="360" y="182" />
              <mxPoint x="940" y="182" />
            </Array>
          </mxGeometry>
        </mxCell>

        <!-- ========== LEGEND ========== -->
        <mxCell id="legend_title" value="&lt;b&gt;Legend&lt;/b&gt;" style="text;html=1;fontSize=12;fontStyle=1;strokeColor=none;fillColor=none;" vertex="1" parent="1">
          <mxGeometry x="120" y="490" width="80" height="20" as="geometry" />
        </mxCell>
        <mxCell id="legend_auth" value="Auth Service" style="rounded=1;fillColor=#dae8fc;strokeColor=#006EAF;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="120" y="516" width="100" height="24" as="geometry" />
        </mxCell>
        <mxCell id="legend_task" value="Task Service" style="rounded=1;fillColor=#e1d5e7;strokeColor=#6a0dad;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="230" y="516" width="100" height="24" as="geometry" />
        </mxCell>
        <mxCell id="legend_log" value="Log Service" style="rounded=1;fillColor=#ffe6cc;strokeColor=#b46504;fontSize=10;" vertex="1" parent="1">
          <mxGeometry x="340" y="516" width="100" height="24" as="geometry" />
        </mxCell>
        <mxCell id="legend_jwt" value="JWT flow" style="text;html=1;fontSize=10;strokeColor=#006400;fontColor=#006400;fontStyle=3;" vertex="1" parent="1">
          <mxGeometry x="120" y="546" width="60" height="20" as="geometry" />
        </mxCell>
        <mxCell id="legend_jwt_line" value="" style="edgeStyle=orthogonalEdgeStyle;dashed=1;strokeColor=#006400;strokeWidth=2;" edge="1" parent="1">
          <mxGeometry relative="1" as="geometry">
            <mxPoint x="186" y="556" as="sourcePoint" />
            <mxPoint x="240" y="556" as="targetPoint" />
          </mxGeometry>
        </mxCell>

      </root>
    </mxGraphModel>
  </diagram>
</mxfile>


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
