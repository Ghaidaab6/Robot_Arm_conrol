# Robot_Arm_control

# Robot Arm Controller –Flutter+PHP 

This project provides a simple control panel for a robot arm with 4 motors. It includes:

* **Flutter App** (UI sliders, pose saving, run/reset)
* **PHP backend scripts** to fetch and update poses
                                                                

---

## Features

* Adjust 4 motors via sliders
* Save multiple poses locally in the app
* Load saved poses with play button
* Run and Reset buttons
* Polls backend every 2 seconds for run instructions
* Backend scripts provide/run poses and reset status

---

## Requirements

* Flutter SDK (>=3.4.0)
* PHP 7.4+ with PDO MySQL
* MySQL 5.7+/MariaDB
* Android device with USB host support

---

## Setup Instructions



###  Server Setup

1. Copy PHP files into your web server root:

   * `server/get_run_pose.php`
   * `server/update_status.php`

2. Edit DB credentials in both PHP files:

```php
$user = '';
$pass = '';
$db   = 'robot_arm';
```

3. Test by opening:

```
http://192.168.100.6/robot/get_run_pose.php
http://192.168.100.6/robot/update_status.php
```

### 3. Flutter App Setup

1. Update `lib/config.dart` with your server URL:

```dart
const String apiBase = 'http://192.168.100.6/robot';
```

2. Install dependencies:

```bash
flutter pub get
```

3. Run on Android device:

```bash
flutter run
```

---

## File Overview

* **lib/main.dart** – entrypoint
* **lib/web.dart** – UI with sliders, saved poses, polling logic
* **lib/config.dart** – API server URL
* **pubspec.yaml** – dependencies (uses `http`)
* **AndroidManifest.xml** – required permissions (`USB`, `INTERNET`)
* **db/schema.sql** – MySQL schema
* **server/get\_run\_pose.php** – returns `{ run, pose }`
* **server/update\_status.php** – sets run flag to `0`

---

## API Endpoints

* **GET** `/get_run_pose.php` → `{ "run": 0|1, "pose": {"m1":..,"m2":..,"m3":..,"m4":..} }`
* **POST** `/update_status.php` → `{ "ok": true }`

---

## How It Works

* Flutter polls `get_run_pose.php` every 2s
* If `run=1`, app updates sliders to that pose
* Pressing **Run** calls `update_status.php` to set `run=0`
* Saved poses are local only (not pushed to DB)

---

## Permissions (Android)

```xml
<uses-feature android:name="android.hardware.usb.host" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_USB" />
```

---

## Notes
* Ensure your device and server are on the same network
* Update firewall settings to allow access to PHP endpoints
* You can extend backend to insert new poses directly into DB
