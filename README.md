# Office Connect & Office Connect Admin

## Overview

**Office Connect** is a Mobile Device Management (MDM)-like solution designed to control Android TV OS devices (used in buses) remotely through an Android mobile app called **Office Connect Admin**. Communication between the client and admin app is achieved via **OneSignal push notifications**, and all interactions with devices are handled via a local server (Raspberry Pi) over the local bus Wi-Fi network.

---

## ✨ Features

### Office Connect (Client App - Android TV)
- First-time setup with permission handling (Notification, Location, Storage).
- Manual registration with auto-filled device info (Model, Android Version, Android ID, IP, SSID, etc.).
- Registers device in cloud DB using Android ID.
- Registers for OneSignal notifications with two keys:
  - Bus Server ID
  - `BusServerId_AndroidId`
- Listens for admin commands and communicates with the local server via Retrofit API.
- Background service handles:
  - Screen streaming using Media Projection API and Socket.IO
  - Command execution via ADB on server
  - Auto-re-registration on network changes
  - Periodic device status updates every 15 minutes

#### Supported Admin Commands:
- `STATUS`: Sends device status to Admin
- `NOTIFY`: Notify Admin when device comes online
- `NAVIGATION`, `BULK_NAVIGATION`: Navigate device remotely
- `LAUNCH_APP`, `INSTALL_APP`, `UNINSTALL_APP`: App management
- `REBOOT`: Restart device
- `START_SCREENING`, `STOP_SCREENING`: Start/Stop screen sharing

---

### Office Connect Admin (Mobile App)
- Admin login with common credentials.
- Displays list of bus operators and their buses.
- Fetches and displays registered devices under selected bus.
- Shows last known device status.
- Sends `STATUS` command to get live status from all devices.
- Sends individual `NOTIFY` to offline devices.
- Displays list of notified devices when they come online.
- Receives live device data via OneSignal and updates UI.
- Displays real-time screen of client devices using streamed bytes from Socket.IO.

---

## 🧠 Architecture

- **MVVM** architecture with Hilt for Dependency Injection.
- **WorkManager** for background tasks.
- **Retrofit 2** for all server communication (local + cloud).
- **Room Database** for caching devices locally and updating UI efficiently.
- **BroadcastReceiver** for power on/off and server monitoring.
- **DataStore Preferences** for storing persistent device data.
- **Media Projection + Socket.IO** for screen streaming.

---

## 🧪 Technologies Used

| Component              | Technology       |
|------------------------|------------------|
| Language               | Kotlin            |
| UI Framework           | Jetpack Compose / Material Theme |
| Dependency Injection   | Hilt              |
| Background Tasks       | WorkManager       |
| Network                | Retrofit 2        |
| Push Notifications     | OneSignal         |
| Local Database         | Room              |
| Local Storage          | DataStore Preferences |
| Real-time Communication| Socket.IO         |
| Screen Capture         | Media Projection  |
| Device Control         | ADB via Raspberry Pi |

---

## 🌐 Communication Flow

1. **Admin App** sends a push notification to all devices registered with a specific Bus Server ID via OneSignal.
2. **Client App** listens for these commands and communicates with the local server using Retrofit.
3. Local server (Raspberry Pi) uses ADB over IP to control devices based on the received command.
4. Screen data is sent from the Client to Server via Socket.IO, and then to the Admin for live screen view.

---

## 🛠 Server Setup

- **Raspberry Pi** acts as a local server inside the bus.
- Executes ADB commands on devices using IP addresses.
- Hosts Socket.IO server for screen streaming.
- Communicates with cloud for device registration and updates.

---

## 📱 Screenshots


---

## 📌 Notes

- Designed for **offline bus environments** using only **local Wi-Fi and Raspberry Pi**.
- Ensures **reliable control of each device per seat**.
- Handles **device boot, network change, offline conditions** gracefully.
- Highly scalable and secure with cloud-device sync and offline-first architecture.

---

## 🤝 Credits

Developed by: **Shashidhar**  
Company: **Jumpstart Technologies Pvt Ltd**  
Role: **Android Developer**  
