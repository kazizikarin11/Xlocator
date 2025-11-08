=
# 🚌 Xlocator (SmartBus Tracker)  
### _Solar-Powered Bus Monitoring, Tracking, and Passenger Reminder System_

---

## 📘 **Project Overview**

**SmartBus Tracker** is a next-generation **IoT-based bus tracking and management system** developed for **St. Mahamandal buses**.  
It integrates **solar-powered tracking devices** with an advanced **software platform** that enables administrators to monitor fleets in real-time, manage routes and schedules, and help passengers locate buses, view ETAs, and receive alerts when their bus approaches their desired stop or location.

This project promotes **sustainability, transparency, and convenience** by combining renewable energy, IoT, and smart mobility solutions.

---

## ⚡ **Core Objectives**

- Track every bus in real time using GPS + IoT modules.  
- Provide a solar-powered device for continuous operation.  
- Offer admin dashboard for route, stop, and fleet management.  
- Provide passengers with live bus location and ETA.  
- Allow passengers to **set reminders** for their desired stops or custom locations.  
- Analyze performance metrics (on-time %, idle times, route adherence).  

---

## ☀️ **Hardware Overview (Device Unit)**

| Component | Function | Specification |
|------------|-----------|---------------|
| **Microcontroller** | Main control unit | ESP32 / STM32 |
| **GPS Module** | Location tracking | u-blox NEO-6M / Quectel L76 |
| **Cellular Module** | Internet connectivity | LTE Cat-1 / NB-IoT / 4G |
| **Accelerometer** | Motion detection | MPU6050 |
| **Power Supply** | Dual source | Bus battery + Solar panel |
| **Solar Panel** | Energy generation | 20W (12V) |
| **Battery Backup** | Power storage | 12V 10Ah LiFePO4 |
| **Charge Controller** | Power regulation | MPPT controller |
| **Antenna System** | Signal reception | External GPS + GSM antenna |
| **Enclosure** | Protection | IP65-rated waterproof casing |

---

## 🔋 **Power System Calculation**

| Parameter | Value |
|------------|--------|
| Device power consumption | 1.9 W |
| Daily energy use | 45.6 Wh/day |
| Solar availability | 5 hours/day |
| Required solar panel | ≈ 9.12 W |
| Recommended panel | 20 W (with margin) |
| Required battery (12V) | ≈ 7.6 Ah |
| Recommended battery | 12V 10Ah |

✅ Ensures continuous 24x7 operation even on cloudy days.

---

## 🧠 **System Architecture**

### 1️⃣ **Device Layer**
- Collects GPS coordinates, time, speed, heading, and sensor data.
- Transmits data via 4G/NB-IoT to cloud using MQTT/HTTPS.
- Supports offline data storage (SD card) and retransmission.
- Solar-charged internal battery powers the device continuously.

### 2️⃣ **Communication Layer**
- Protocol: MQTT / HTTPS (TLS encrypted)
- Message Frequency: 10–30 seconds while moving, 60–300 seconds when idle.
- Payload Format: JSON / CBOR
- Secure Authentication using device tokens.

### 3️⃣ **Backend Layer**
- Built with **Node.js / FastAPI**.
- Uses **PostgreSQL / TimescaleDB** for time-series data.
- Real-time communication handled by **WebSocket / MQTT broker**.
- Implements business logic: route management, ETA prediction, reminder handling, and analytics.

### 4️⃣ **Frontend Layer**
- **Admin Dashboard** (React.js + Mapbox/Leaflet)
- **Passenger Web & Mobile App** (React Native / Flutter)
- Features: real-time map, route list, bus details, stop schedules, and alert setup.

---

## 🗂️ **Database Schema (Simplified)**

| Table | Description | Key Fields |
|--------|--------------|------------|
| **devices** | Bus tracking devices | id, imei, bus_no, sim_no, last_seen, status |
| **routes** | Bus routes and schedules | id, name, start_stop, end_stop, timetable |
| **stops** | Stops linked to routes | id, name, lat, lon, route_id |
| **telemetry** | GPS + sensor data | id, device_id, lat, lon, speed, battery, timestamp |
| **users** | Registered admin/passenger users | id, name, role, email, password_hash |
| **reminders** | Passenger-set alerts | id, user_id, bus_id, stop_id, lat, lon, radius_m, triggered |

---

## 💡 **Core Functional Modules**

### 🚌 Bus Tracking
- Real-time GPS map view.
- Live position, speed, direction.
- Route-wise and bus-wise filters.

### 🗺️ Route & Stop Management
- Add/edit routes and stops.
- Set stop coordinates and scheduled times.
- Auto-detect bus arrival/departure using geofencing (30–50m radius).

### ⏱️ ETA & Delay Estimation
- ETA = Distance / Avg Speed.
- Dynamic updates using live traffic & bus telemetry.
- Delay alerts if ETA > schedule threshold.

### 🔔 Passenger Reminder System
- Users can set reminders for:
  - Specific bus & stop, or  
  - Custom pinned location.
- Choose alert radius (e.g., 500m, 1km) or time (e.g., 5 minutes before arrival).
- Backend checks distance every GPS update:
  ```python
  if haversine(bus.lat, bus.lon, reminder.lat, reminder.lon) <= reminder.radius_m:
      trigger_notification(user)

Notifications via Firebase Cloud Messaging (Push) or SMS fallback.

Reminder auto-deactivates after trigger or trip completion.


📊 Reports & Analytics

On-time performance (% punctual trips).

Stop dwell time & route deviation stats.

Energy efficiency and uptime reports.



---

🔐 Security Features

TLS encryption for all MQTT/HTTP traffic.

Device-specific authentication tokens.

Secure boot & signed OTA firmware updates.

Encrypted user passwords (bcrypt).

Role-based access control (Admin / Dispatcher / Passenger).



---

🧰 Technology Stack

Layer	Technology

Firmware	C++ / MicroPython (ESP32 / STM32)
Backend API	Node.js (Express) / Python (FastAPI)
Database	PostgreSQL + TimescaleDB
Realtime Broker	EMQX / Mosquitto (MQTT)
Frontend Dashboard	React.js + Leaflet / Mapbox
Mobile App	React Native / Flutter
Notifications	Firebase Cloud Messaging (FCM)
Cloud Hosting	AWS / DigitalOcean / GCP
Authentication	JWT-based secure login



---

🧾 Example API Payloads

➤ Device → Server (Telemetry)

{
  "device_id": "BUS_102",
  "lat": 18.5204,
  "lon": 73.8567,
  "speed": 42.5,
  "battery_voltage": 12.3,
  "timestamp": "2025-10-20T09:45:00Z"
}

➤ Passenger → Server (Set Reminder)

{
  "user_id": 12,
  "bus_id": "BUS_102",
  "stop_id": 14,
  "radius_m": 1000
}


---

🗓️ Development Roadmap

Phase	Duration	Deliverables

Phase 1	2 Weeks	Prototype circuit design, solar + battery testing
Phase 2	4 Weeks	Firmware development, GPS → Cloud telemetry
Phase 3	4 Weeks	Backend APIs, MQTT integration, live map dashboard
Phase 4	3 Weeks	Passenger app + reminder feature implementation
Phase 5	2 Weeks	Field testing on sample 5 buses
Phase 6	Continuous	Deployment, maintenance, analytics optimization



---

📈 Expected Benefits

✅ Live visibility of all buses.

✅ Accurate ETAs for passengers.

✅ Better schedule adherence and punctuality.

✅ Reduced energy cost via solar integration.

✅ Enhanced passenger experience with reminders.

✅ Scalable architecture for city-wide adoption.



---

💬 Future Enhancements

AI-based ETA prediction using real traffic data.

Integration with smart bus stops and digital signboards.

NFC-based smart ticketing.

Audio alerts for visually impaired passengers.

Fleet fuel and emission monitoring.

Integration with municipal open data portals.



---

👨‍💻 Author & Maintainer

Name: Azzam Anas Ahmed Hasan
Role: Founder & Project Lead
Focus Areas: Concept Development, System Architecture, IoT Integration, and Software Design.


---

🧾 License

This project is licensed under the MIT License — free for modification and distribution with proper credits.


---

🌟 Tagline

> “Powering Public Transport with Sunlight and Smart Intelligence.”




---

🧱 Suggested Folder Structure

SmartBus-Tracker/
│
├── backend/
│   ├── api/
│   ├── models/
│   ├── routes/
│   ├── utils/
│   └── server.js
│
├── firmware/
│   ├── main.ino
│   ├── config.h
│   └── ota_update.cpp
│
├── frontend/
│   ├── public/
│   ├── src/
│   ├── components/
│   ├── pages/
│   └── App.jsx
│
├── mobile_app/
│   ├── src/
│   ├── screens/
│   └── App.js
│
├── docs/
│   ├── architecture_diagram.png
│   ├── power_calculations.pdf
│   └── api_reference.md
│
└── README.md


---

❤️ Contributions

Contributions are welcome!

Fork this repository

Create your feature branch (git checkout -b feature-name)

Commit changes (git commit -m "Added new feature")

Push to branch (git push origin feature-name)

Create a Pull Request



---

🏁 Final Note

SmartBus Tracker aims to revolutionize public transport tracking through renewable power and intelligent automation — bringing comfort, reliability, and efficiency to everyday commuters.


---

