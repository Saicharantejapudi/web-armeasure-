# Web-ARMeasure 📏

A lightweight, zero-install Augmented Reality (AR) tape measure for the web, powered by **Three.js** and the **WebXR Device API**. Run precise linear distance and vertical height measurements directly from your mobile browser.

![Web-ARMeasure](https://img.shields.io/badge/WebXR-Immersive--AR-blue?style=flat-square)
![Three.js](https://img.shields.io/badge/Three.js-r160-black?style=flat-square&logo=three.js)
![TailwindCSS](https://img.shields.io/badge/TailwindCSS-v3-38bdf8?style=flat-square&logo=tailwind-css)

---

## ✨ Features

- **Linear Measurement (`mode: linear`)**: Measure point-to-point 3D distance between any two surfaces with real-time distance preview.
- **Height Measurement (`mode: height`)**: Measure the vertical height of objects (walls, doors, furniture, people) from a base plane using ray-plane intersection.
- **Real-Time Unit Conversion**: Instantly switch between Metric (`cm`, `m`) and Imperial (`in`, `ft`) units.
- **Surface Hit-Testing**: Anchors lock onto real-world planes using WebXR Viewer Space hit-testing with visual reticle feedback.
- **Haptic Feedback**: Tactile confirmation on point placement and resets (supported devices).
- **Glassmorphic Mobile HUD**: Clean, high-contrast user interface adapted for mobile notches and safe areas.
- **Desktop 3D Simulator**: Interactive fallback mode allowing preview and testing directly on desktop or non-AR devices.

---

## 📱 Hardware & Browser Requirements

WebXR Augmented Reality requires AR tracking hardware and an SSL-secured connection:

- **Android**: Google Chrome with [Google Play Services for AR (ARCore)](https://play.google.com/store/apps/details?id=com.google.ar.core) installed.
- **iOS**: WebXR-compatible browser (such as WebXR Viewer).
- **HTTPS Connection**: WebXR Device APIs are strictly disabled on insecure origins (HTTP). Local testing requires `localhost` or an HTTPS tunnel/server.

---

## 🚀 Quick Start & Local Testing

### 1. Serve Locally with HTTPS or Localhost

You can run any local web server. For mobile testing over the same Wi-Fi network, HTTPS is required:

#### Option A: Using `npx serve` (Localhost)
```bash
npx -y serve .
```

#### Option B: Using Python
```bash
python -m http.server 8000
```

### 2. Testing on an Android Device via Chrome USB Debugging

To run on a real phone with `localhost` security context:
1. Connect your Android phone to your computer via USB with **USB Debugging** enabled.
2. Open Chrome on your computer and navigate to:
   ```
   chrome://inspect/#devices
   ```
3. Under **Port Forwarding**, forward port `8000` to `localhost:8000`.
4. On your mobile Chrome browser, open `http://localhost:8000` — Chrome treats `localhost` as a secure context, enabling WebXR without SSL certificates!

---

## 📐 How the Measurement Logic Works

### Linear Mode
Calculates the Euclidean distance in 3D world space between two anchored positions:
$$\text{Distance} = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2 + (z_2 - z_1)^2}$$

### Height Mode
Height mode projects a camera ray through a vertical plane aligned with the base point to accurately gauge vertical height ($\Delta y$) without needing a physical ceiling plane to hit-test against:
1. When **Base Point** is placed, an anchor is locked on the ground/surface.
2. When aiming upwards, a vertical plane $P$ is formed at the base anchor facing the camera.
3. The camera's forward ray intersects plane $P$, producing the elevated top point:
   $$\text{Height} = \max(0, y_{\text{intersect}} - y_{\text{base}})$$

---

## 🛠️ Architecture

```
web-armeasure-/
├── index.html        # Main application (HTML5, Three.js engine, WebXR loop, HUD)
├── .gitignore        # Git ignore rules for OS, build, and editor files
└── README.md         # Documentation & setup guide
```

---

## 📄 License

MIT License. Feel free to use and modify for personal or commercial projects.