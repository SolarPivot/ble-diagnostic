# SolarPivotPower BLE Diagnostic Tool

A zero-install browser-based Bluetooth Low Energy (BLE) diagnostic tool for **pSolBot** devices. Built for field support and diagnostics — no app store, no installation, just open a link in Chrome.

**Live tool:** [solarpivot.github.io/ble-diagnostic](https://solarpivot.github.io/ble-diagnostic)

---

## Overview

This tool allows support staff and technically capable customers to connect directly to a pSolBot device over BLE, send commands, view real-time responses, and capture a full event log for diagnostic purposes — all from a Chrome browser tab on an Android device.

It is intended as a **support and diagnostics companion** to the native pSolBot apps available on the Google Play and Apple App Stores. It is not a replacement for the native apps.

---

## Features

- **BLE scan & connect** — connects to pSolBot devices using the Web Bluetooth API
- **Auto device identification** — automatically queries the device on connection and displays Name, Serial Number, Software version, Hardware version, and AutoStart setting
- **Quick commands** — one-tap commands for Track (GPS), SunRise, SolarNoon, and SunSet
- **Track command** — builds and sends a GPS + datetime string using the phone's location and local time/UTC offset
- **Custom command input** — type and send any command string manually
- **Real-time event log** — timestamped log of all connection events, commands sent, and data received from the device
- **Copy log to clipboard** — one tap to copy the full event log, ready to paste into a support email
- **Auto-reconnect** — automatically attempts to reconnect if the BLE link drops
- **Connection health monitor** — detects silent disconnects (e.g. ESP32 reset) via periodic health checks
- **Drop counter & uptime timer** — tracks connection stability during a diagnostic session
- **Wrong device warning** — alerts the user if the connected device name doesn't match expected pSolBot naming

---

## Platform Compatibility

| Platform | Support |
|---|---|
| Android + Google Chrome | ✅ Full support |
| Windows + Google Chrome | ✅ Supported (GPS unavailable on most PCs) |
| macOS + Google Chrome | ✅ Supported (GPS unavailable on most Macs) |
| iOS (iPhone / iPad) | ❌ Not supported — Apple restricts Web Bluetooth on iOS |
| Safari (any platform) | ❌ Not supported |
| Firefox (any platform) | ❌ Not supported |

> **Recommended:** Android phone or tablet running Google Chrome.

---

## Usage

1. Open [solarpivot.github.io/ble-diagnostic](https://solarpivot.github.io/ble-diagnostic) in Chrome on Android
2. Allow location permission when prompted (required by Chrome for BLE scanning)
3. Tap **Scan** and select your pSolBot device from the browser popup
4. The tool connects automatically and queries the device — Name, S/N, SW, HW, and AutoStart populate in the device panel
5. Use **Track** to send your GPS coordinates and local time to the device
6. Use **SunRise / SolarNoon / SunSet** under Manual Control to send sun position commands
7. When done, tap **Copy** under Event Log and paste the log into an email to [support@solarpivotpower.com](mailto:support@solarpivotpower.com)

---

## ESP32 Requirements

For full functionality (including receiving data back from the device), the ESP32 firmware must:

1. **Set MTU before init** — `BLEDevice::setMTU(150)` must be called before `BLEDevice::init()`
2. **Add CCCD descriptor to the output characteristic** — required for Web Bluetooth notifications:

```cpp
#include <BLE2902.h>

pOutputChar = pService->createCharacteristic(
  CHARACTERISTIC_OUTPUT_UUID,
  BLECharacteristic::PROPERTY_READ | BLECharacteristic::PROPERTY_NOTIFY
);
pOutputChar->addDescriptor(new BLE2902());  // required for Web Bluetooth
```

Without `BLE2902`, Chrome will refuse to subscribe to notifications and incoming data will not be displayed.

---

## BLE UUIDs

```
Service:            f5430ca7-9d13-4140-8834-6fa5cfb10f44
Input Characteristic  (write): 1e10a180-dba0-4667-850f-08bb444a8256
Output Characteristic (notify): 42ebb4ff-bacf-482b-9eff-f69df2d7409d
```

---

## Command Reference

| Button | Command Sent | Description |
|---|---|---|
| Track | `2\|{lon}\|{lat}\|{datetime}\|1\|#` | GPS coordinates + local datetime |
| SunRise | `345#` | Move to sunrise position |
| SolarNoon | `390#` | Move to solar noon position |
| SunSet | `3135#` | Move to sunset position |
| Status (auto) | `7#` | Query device status — sent automatically on connect |

**Track command format example:**
```
2|-97.74306|30.26715|09/27/2023 07:39:37, -0500|1|#
```

**Status response format:**
```
pSolBot 1, 00000925, 0, 1.07, 0101
         ^name      ^serial  ^autostart(mins)  ^sw  ^hw
```

---

## Project Structure

```
ble-diagnostic/
└── index.html      # Single self-contained file — all HTML, CSS, and JS
```

All assets (including the SolarPivotPower logo) are embedded as base64 inside `index.html`. No external dependencies, no build step, no framework. The entire tool is one file.

---

## Deployment

Hosted via GitHub Pages from the `main` branch. Any push to `main` deploys automatically within ~30 seconds.

To update: edit `index.html`, commit, and push. The live tool updates automatically.

---

## Version History

| Version | Notes |
|---|---|
| v1.3 | Enhanced diagnostic logging — scan tap, cached vs fresh device, GATT state, service discovery timing, Chrome tip on failure |
| v1.2 | GATT connection guard — prevents "GATT Server disconnected" error on cached devices |
| v1.1 | Clear device info panel on disconnect; shared clearDeviceUI() |
| v1.0 | Auto device status on connect; device info panel; restructured quick commands |
| v0.9 | Upfront GPS permission request; Track delimiter fix (`\|1\|#`) |
| v0.8 | SPP logo transparent background; header layout (logo left, title right) |
| v0.7 | Help buttons on all sections; wrong device warning |
| v0.6 | SPP logo added to header |
| v0.5 | Uptime timer and drop counter replace fake RSSI |
| v0.4 | Quick commands renamed; Track command with GPS + UTC offset |
| v0.3 | Output characteristic (notify) for received data; BLE2902 fix documented |
| v0.2 | GATT notification error suppressed; duplicate device fix |
| v0.1 | Initial release |

---

## Support

For device issues or diagnostic log submissions:
**support@solarpivotpower.com**

&copy; SolarPivotPower. All rights reserved.
