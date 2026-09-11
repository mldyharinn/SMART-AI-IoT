# Smart Home IoT — Project Framework

## 1. Project Overview

Project ini merupakan sistem **Smart Home berbasis IoT** yang menghubungkan:

- **Frontend Web** untuk dashboard dan kontrol perangkat.
- **Python Backend** sebagai pusat logika aplikasi dan API.
- **MQTT Broker** sebagai media komunikasi IoT.
- **ESP32** sebagai perangkat IoT yang membaca sensor dan mengontrol actuator.
- **Database** untuk menyimpan data pengguna, perangkat, dan histori sensor.
- **GitLab** sebagai repository dan collaboration platform tim.

---

## 2. System Architecture

```text
                         SMART HOME SYSTEM
                                │
              ┌─────────────────┴─────────────────┐
              │                                   │
              ▼                                   ▼
        WEB FRONTEND                         DATABASE
     HTML + Tailwind CSS                    MySQL/MongoDB
       + JavaScript
              │
              │ HTTP / WebSocket
              ▼
       ┌──────────────────┐
       │  PYTHON BACKEND   │
       │ FastAPI / Flask   │
       │                  │
       │ REST API         │
       │ MQTT Client      │
       │ Authentication   │
       │ Business Logic   │
       └────────┬─────────┘
                │
                │ MQTT
                ▼
       ┌──────────────────┐
       │   MQTT BROKER     │
       │ Mosquitto / etc.  │
       └────────┬─────────┘
                │
                │ MQTT
                ▼
       ┌──────────────────┐
       │      ESP32        │
       │                  │
       │ Sensors          │
       │ Relay            │
       │ LED              │
       │ Actuators        │
       └──────────────────┘
```

---

## 3. Frontend Framework

Frontend bertanggung jawab terhadap **tampilan, interaksi pengguna, dan komunikasi dengan backend**.

### Technology

```text
HTML
  ↓
Tailwind CSS
  ↓
JavaScript
  ↓
REST API / WebSocket
```

### Struktur

```text
frontend/
├── index.html
├── dashboard.html
├── devices.html
├── analytics.html
├── settings.html
├── css/
├── js/
│   ├── api.js
│   ├── dashboard.js
│   ├── devices.js
│   └── websocket.js
├── components/
└── assets/
    ├── images/
    ├── icons/
    └── logo/
```

### Tanggung Jawab

- Menampilkan dashboard.
- Menampilkan status perangkat.
- Menampilkan data sensor.
- Kontrol ON/OFF perangkat.
- Menampilkan notifikasi.
- Responsive design.
- Mengambil data dari REST API.
- Menerima update real-time melalui WebSocket.

---

## 4. Backend Framework

Backend menggunakan **Python**.

Framework yang direkomendasikan:

```text
FastAPI
```

FastAPI bertanggung jawab sebagai penghubung antara frontend, database, MQTT, dan sistem IoT.

### Struktur

```text
backend/
├── app/
│   ├── main.py
│   ├── config.py
│   │
│   ├── api/
│   │   ├── auth.py
│   │   ├── devices.py
│   │   └── sensors.py
│   │
│   ├── mqtt/
│   │   ├── client.py
│   │   ├── publisher.py
│   │   └── subscriber.py
│   │
│   ├── models/
│   ├── schemas/
│   ├── services/
│   └── database/
│
├── requirements.txt
└── .env
```

### Tanggung Jawab Backend

- REST API.
- Authentication.
- Validasi data.
- Business logic.
- MQTT publish.
- MQTT subscribe.
- WebSocket.
- Database access.
- Monitoring device.
- Logging dan error handling.

---

## 5. MQTT Communication

MQTT digunakan sebagai protokol komunikasi antara backend dan ESP32.

### Contoh Topic

```text
smart-home/device/{device_id}/command
smart-home/device/{device_id}/status
smart-home/sensor/{device_id}/temperature
smart-home/sensor/{device_id}/humidity
smart-home/sensor/{device_id}/light
```

### Contoh Command

Frontend mengirim:

```text
POST /api/devices/light
```

Backend kemudian publish:

```text
Topic:
smart-home/device/living-room/command
```

Payload:

```json
{
  "device": "light",
  "action": "ON"
}
```

ESP32 menerima command tersebut dan mengaktifkan relay.

---

## 6. ESP32 Framework

ESP32 menjadi perangkat IoT di lapangan.

### Struktur

```text
esp32/
├── src/
│   └── main.cpp
├── include/
├── lib/
└── platformio.ini
```

Jika menggunakan Arduino IDE, kode utama dapat berada pada:

```text
esp32/
└── smart_home_esp32.ino
```

### Tanggung Jawab ESP32

- Membaca sensor.
- Mengontrol relay/actuator.
- Terhubung ke Wi-Fi.
- Terhubung ke MQTT Broker.
- Subscribe command.
- Publish status.
- Reconnect ketika koneksi terputus.

---

## 7. Database

Database digunakan untuk menyimpan data aplikasi.

Contoh data:

```text
users
devices
sensor_readings
device_logs
automation_rules
```

Contoh:

```text
users
├── id
├── name
├── email
└── password_hash

devices
├── id
├── name
├── type
├── status
└── location

sensor_readings
├── id
├── device_id
├── temperature
├── humidity
├── value
└── timestamp
```

---

## 8. Data Flow

### A. Kontrol Perangkat

```text
User
 ↓
Frontend
 ↓
REST API
 ↓
Python Backend
 ↓
MQTT Publish
 ↓
MQTT Broker
 ↓
ESP32
 ↓
Relay / Actuator
 ↓
Device
```

### B. Data Sensor

```text
Sensor
 ↓
ESP32
 ↓
MQTT Publish
 ↓
MQTT Broker
 ↓
Python Backend
 ↓
Database
 ↓
WebSocket / REST API
 ↓
Frontend Dashboard
```

---

## 9. Real-Time Communication

Untuk data yang membutuhkan update langsung:

```text
ESP32
  ↓
MQTT
  ↓
Backend
  ↓
WebSocket
  ↓
Frontend
```

Contoh:

```text
Temperature: 28°C
```

Ketika sensor berubah:

```text
28°C → 29°C
```

Dashboard dapat berubah tanpa melakukan refresh halaman.

---

## 10. GitLab Team Workflow

GitLab digunakan untuk version control dan kolaborasi.

### Repository

```text
smart-home-iot/
├── frontend/
├── backend/
├── esp32/
├── docs/
├── README.md
└── .gitignore
```

### Branch

```text
main
│
├── feature/frontend
├── feature/backend
├── feature/mqtt
└── feature/esp32
```

### Workflow

```text
Clone
  ↓
Create Branch
  ↓
Pull Latest Code
  ↓
Coding
  ↓
Testing
  ↓
git add
  ↓
git commit
  ↓
git push
  ↓
Merge Request
  ↓
Code Review
  ↓
Merge
  ↓
main
```

### Aturan

- Jangan bekerja langsung di `main`.
- Gunakan branch berdasarkan fitur.
- Commit harus jelas.
- Pull sebelum mulai pekerjaan.
- Buat Merge Request setelah fitur selesai.
- Review kode sebelum merge.
- `main` harus selalu dalam kondisi stabil.

---

## 11. Recommended Team Division

Contoh pembagian tim:

| Role | Responsibility |
|---|---|
| Frontend Developer | UI, Tailwind, JavaScript, API integration |
| Backend Developer | FastAPI, REST API, database |
| IoT Developer | ESP32, sensor, actuator |
| MQTT/Integration | MQTT broker, topic, communication |
| UI/UX | Figma, design system, usability |

Satu orang dapat memegang lebih dari satu role jika jumlah anggota terbatas.

---

## 12. Development Environment

### Frontend

```text
HTML
Tailwind CSS
JavaScript
```

### Backend

```text
Python 3.x
FastAPI
Uvicorn
Paho MQTT
SQLAlchemy / database driver
```

### IoT

```text
ESP32
Arduino IDE / PlatformIO
MQTT
Wi-Fi
```

### Infrastructure

```text
MQTT Broker
Database
GitLab
```

---

## 13. Environment Variables

Credential dan konfigurasi penting **jangan disimpan langsung di source code**.

Contoh `.env`:

```env
MQTT_BROKER=localhost
MQTT_PORT=1883
MQTT_USERNAME=
MQTT_PASSWORD=

DATABASE_URL=

SECRET_KEY=
```

Tambahkan `.env` ke `.gitignore`:

```gitignore
.env
__pycache__/
*.pyc
node_modules/
```

---

## 14. Development Stages

### Phase 1 — UI/UX

```text
Figma
 ↓
Design System
 ↓
Frontend Layout
```

### Phase 2 — Frontend

```text
HTML
 ↓
Tailwind CSS
 ↓
JavaScript
 ↓
Dummy Data
```

### Phase 3 — Backend

```text
FastAPI
 ↓
REST API
 ↓
Database
```

### Phase 4 — MQTT

```text
MQTT Broker
 ↓
Python MQTT Client
 ↓
ESP32 MQTT Client
```

### Phase 5 — Integration

```text
Frontend
 ↕
Backend
 ↕
MQTT
 ↕
ESP32
```

### Phase 6 — Testing

```text
Unit Testing
 ↓
Integration Testing
 ↓
Device Testing
 ↓
UI/UX Testing
 ↓
Final Testing
```

### Phase 7 — Deployment

```text
GitLab
 ↓
Production Server
 ↓
Backend
 ↓
MQTT Broker
 ↓
ESP32
```

---

## 15. Final Architecture

```text
                       👤 USER
                          │
                          ▼
                 ┌─────────────────┐
                 │     FRONTEND    │
                 │ HTML            │
                 │ Tailwind CSS    │
                 │ JavaScript      │
                 └────────┬────────┘
                          │
                  HTTP / WebSocket
                          │
                          ▼
                 ┌─────────────────┐
                 │ PYTHON BACKEND  │
                 │    FastAPI      │
                 │ REST API        │
                 │ MQTT Client     │
                 │ Business Logic  │
                 └───────┬─┬───────┘
                         │ │
              Database  │ │ MQTT
                         │ │
                         ▼ ▼
                  ┌──────────────┐
                  │ MQTT BROKER  │
                  └───────┬──────┘
                          │
                         MQTT
                          │
                          ▼
                 ┌─────────────────┐
                 │      ESP32      │
                 │    Sensors      │
                 │    Relay        │
                 │    Actuator     │
                 └─────────────────┘

                  GITLAB
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
    Frontend       Backend        ESP32
     Branch         Branch         Branch
       │             │             │
       └─────────────┼─────────────┘
                     ▼
                   main
               FINAL PROJECT
```

---

## 16. Prinsip Utama Project

> **Frontend menangani tampilan dan interaksi pengguna.**  
> **Backend menangani logic dan API.**  
> **MQTT menangani komunikasi IoT.**  
> **ESP32 menangani sensor dan actuator.**  
> **Database menyimpan data.**  
> **GitLab mengelola kode dan kolaborasi tim.**

Arsitektur utama:

```text
Frontend
    ↕
Python Backend
    ↕
MQTT Broker
    ↕
ESP32
```

GitLab berada sebagai **sistem version control dan collaboration**, bukan sebagai bagian dari jalur komunikasi perangkat.
