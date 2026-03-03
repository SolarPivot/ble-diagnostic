[README.md](https://github.com/user-attachments/files/25670358/README.md)
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

## Deployment

Hosted via GitHub Pages from the `main` branch. Any push to `main` deploys automatically within ~30 seconds.

To update: edit `index.html`, commit, and push. The live tool updates automatically.

---

## Version History

| Version | Notes |
|---|---|
| v1.1 | Clear device info panel on disconnect; shared clearDeviceUI() |
| v1.0 | Initial release |

---

## Support

For device issues or diagnostic log submissions:
**support@solarpivotpower.com**

&copy; SolarPivotPower. All rights reserved.
