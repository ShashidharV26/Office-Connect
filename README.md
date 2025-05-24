🛰️ Office Connect & Office Connect Admin
Office Connect is an MDM (Mobile Device Management)-like system built for Android TV devices inside public transport (buses), enabling remote monitoring and control from a mobile admin app.

This system enables real-time device management over a local bus Wi-Fi network and local server (Raspberry Pi), using OneSignal Push Notifications for communication between the Android TV clients and the admin mobile app.

📲 Applications
🧩 Office Connect (Client - Android TV App)
Installed on Android TV OS devices at each bus seat.

🧑‍💼 Office Connect Admin (Admin - Android Mobile App)
Used by the bus operator/admin to monitor and control devices remotely.

🎯 Features
✅ Client App (Android TV)
Requires permissions (notifications, location, storage) and manual seat registration on first launch.

Auto-populates device info: Android ID, model, IP, SSID, server ID.

Registers device to cloud and OneSignal using unique keys.

Always runs in the background, listens for push commands like:

STATUS – Send device status.

NOTIFY – Notify admin upon reconnection.

NAVIGATION, BULK_NAVIGATION – Remote UI control.

LAUNCH_APP, INSTALL_APP, UNINSTALL_APP, REBOOT.

START_SCREENING, STOP_SCREENING – Live screen streaming to admin.

Sends ADB command requests to local Raspberry Pi server.

Uses MediaProjection + Socket.IO to stream screen.

Auto-registration after reboot and periodic self-update every 15 minutes.

📱 Admin App (Android Mobile)
Secure login using common credentials.

View operators → buses → devices.

Sends commands via OneSignal push using unique IDs.

Fetches real-time live status of devices (STATUS command).

Notified when offline devices reconnect (NOTIFY).

Remotely view screen (live feed) of selected devices.

Navigation and control through local server with ADB.

Displays a Notified Devices screen for later responses.

Smart caching and auto-updating of device statuses.

📡 Communication Architecture
mermaid
Copy
Edit
graph TD
  AdminApp[Office Connect Admin App]
  OneSignal[OneSignal Server]
  Cloud[Cloud Database]
  BusWiFi[Local Bus Wi-Fi]
  Server[Raspberry Pi (Local Server)]
  ClientApp[Office Connect Client App]
  TVDevices[Android TV Devices]

  AdminApp -- Push Commands --> OneSignal
  OneSignal -- Push Notification --> ClientApp
  ClientApp -- API Call (command) --> Server
  Server -- ADB Command --> TVDevices
  ClientApp -- Socket.IO (screen) --> Server
  Server -- Socket.IO (screen) --> AdminApp
  ClientApp -- Status/Data --> Cloud
  AdminApp -- Sync --> Cloud
🛠️ Tech Stack
Component	Technology
Language	Kotlin
Architecture	MVVM
UI	Jetpack Compose, Material Design
Networking	Retrofit2
Notifications	OneSignal
Local Server	Raspberry Pi with ADB
Background Tasks	WorkManager
Dependency Injection	Hilt
Local Storage	DataStore Preferences
Database	Room
Realtime Screen Streaming	MediaProjection + Socket.IO
Lifecycle Awareness	Broadcast Receivers for device status

🔐 Key Functional Highlights
Smart Offline Detection: Ignores outdated commands (over 5 mins).

Auto Recovery: Devices auto-register and update after reboot.

Real-Time Monitoring: View connected TV screens remotely.

Command Routing: ADB commands routed via local server.

Minimal Cloud Usage: Designed for offline-first environments using local bus networks.

📷 Screenshots
(You can add screenshots of both the Client and Admin apps UI here to make it more visual)

📦 Project Structure
perl
Copy
Edit
OfficeConnect/
├── app-client/             # Android TV client code
├── app-admin/              # Android mobile admin app
├── shared/                 # Shared utilities and constants
└── local-server/           # Raspberry Pi server scripts and config
🧪 Installation
📺 For Client (Android TV)
Install the APK.

Grant required permissions.

Enter the seat number and register.

📱 For Admin (Mobile)
Install the APK.

Login with admin credentials.

Select Operator → Bus → Devices.

🔄 Workflow Summary
Admin selects a device or all devices.

Sends a push command via OneSignal.

Client receives command, forwards to local server.

Server executes the corresponding ADB command.

For START_SCREENING, media projection starts and streams to admin.

🤝 Contributions
Currently a closed-source project developed for Touring Talkies Company. If you're interested in similar projects, feel free to contact me.

👨‍💻 Developed By
Shashidhar
Android Developer | Kotlin | MDM Systems | Remote Device Management
