<div align="center">

# 🍎 Apple Health Dashboard

**A privacy-first, single-file HTML dashboard for analyzing your Apple Health export.**

Import the ZIP straight from your iPhone. Get instant visualizations, compare any metrics, run correlation analyses, and export AI-ready reports — all locally in your browser. No server, no upload, no tracking.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![No build step](https://img.shields.io/badge/build-none-brightgreen)](#)
[![Single file](https://img.shields.io/badge/single--file-HTML-blue)](#)
[![Works offline](https://img.shields.io/badge/works-offline-success)](#)

**🌐 [Live demo](https://pit711.github.io/apple-health-dashboard/)** &nbsp;·&nbsp; [Report a bug](https://github.com/pit711/apple-health-dashboard/issues/new?template=bug_report.yml)

</div>

---

## Why this exists

Apple Health collects years of incredibly detailed data about you — but the built-in app only shows you small slices at a time. Third-party tools mostly want you to upload your data to *their* servers. This project takes the opposite approach: **your health data never leaves your device**. The entire dashboard is one HTML file you can save, share, and run anywhere.

## ✨ Features

- **🔒 Local-first** — your health data never leaves your device. No server, no upload, no telemetry.
- **📈 30+ metrics auto-detected** — steps, heart rate, HRV, VO₂max, sleep, weight, blood pressure, workouts, and more
- **⚡ Handles huge exports** — streaming XML parser processes multi-GB files without crashing the browser
- **🧠 Smart deduplication** — when iPhone and Apple Watch both record the same metric, only the higher-priority source is counted (mirroring Apple Health's own internal logic)
- **🔍 8 analysis tabs** — Overview, Activity, Heart & Circulation, Body, Sleep, Compare, Correlation, Raw Data
- **🔀 Multi-metric overlay** — up to 4 metrics on one time axis, each with its own y-axis
- **📐 Correlation analysis** — Pearson coefficient with scatter plot and trend line for any metric pair
- **🖨️ Per-chart printing** — print any single chart for medical appointments
- **🤖 AI export** — produces compact JSON/CSV/Markdown reports (~100–300 KB for 12 months) sized to fit in Claude/ChatGPT context windows
- **📱 Fully responsive** — works on desktop, tablet, and mobile
- **🪶 Single HTML file** — no build, no install, just open

## 🚀 Quick Start

### Option 1: Use the live demo

Just go to **https://pit711.github.io/apple-health-dashboard/** and drop your export there. Nothing is uploaded — the page runs entirely in your browser.

### Option 2: Run it locally

1. Download [`index.html`](index.html) (or clone this repo)
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge)
3. Save it anywhere — works offline after the first load

### Exporting your Apple Health data

On your iPhone:

1. Open the **Health** app
2. Tap your **profile picture** (top right)
3. Scroll down → **Export All Health Data**
4. Wait — it can take a few minutes
5. Share the ZIP to your computer (AirDrop, email, cloud)
6. Drag the ZIP onto the dashboard

## 🤖 Using the AI Export

The dashboard can produce a compact, structured summary of your data, sized specifically to fit inside a Claude or ChatGPT conversation.

1. Load your export in the dashboard
2. Click **"Export für KI"** (top right of the toolbar)
3. Choose period (12 months recommended), format (JSON recommended), and detail level
4. The dialog shows the estimated file size live — green means it fits comfortably
5. Download and drop the file into a Claude or ChatGPT chat with a prompt like:

> *Analyze my Apple Health data. Find trends, anomalies, and correlations between metrics. Suggest things I should pay attention to.*

The export includes summary statistics per metric, a daily/weekly/monthly time series, and all workouts in the period — plus a metadata block that tells the AI exactly how the data was aggregated.

## 🔧 How it works

Apple Health exports a `export.xml` inside a ZIP. For long-term users this XML can easily exceed 1 GB.

1. **Streams the ZIP** chunk by chunk via JSZip's `internalStream` API — never holding the full XML in memory
2. **Parses incrementally** with a stateful regex parser that handles tag boundaries across chunk edges
3. **Aggregates to daily buckets** keyed by metric type and source
4. **Deduplicates by source priority** (Watch > iPhone > others) for cumulative metrics
5. **Renders with Chart.js** — only the active tab's charts are drawn

### Why streaming matters

JavaScript's V8 engine has a maximum string length of ~512 MB. A naive parser that reads the whole XML as one string crashes with `RangeError: Invalid string length` on real-world exports. The streaming approach handles arbitrarily large files.

## 📋 Supported Metrics

| Category | Metrics |
|---|---|
| **Activity** | Steps, walking/running distance, cycling distance, flights climbed, exercise minutes, stand time |
| **Energy** | Active energy, basal energy |
| **Heart** | Heart rate (min/avg/max), resting HR, walking HR, HRV (SDNN), VO₂ max |
| **Vitals** | Blood pressure (sys/dia), oxygen saturation, respiratory rate, body temperature |
| **Body** | Weight, BMI, body fat %, lean body mass, height |
| **Sleep** | Sleep duration, sleep stages |
| **Other** | Blood glucose, dietary water/energy, mindful sessions, audio exposure, walking speed/step length |
| **Workouts** | All workout types with duration, energy, distance |

Missing a metric? [Open an issue](https://github.com/pit711/apple-health-dashboard/issues/new?template=new_metric.yml) — adding new ones takes about 30 seconds.

## 🌐 Browser Compatibility

| Browser | Minimum version |
|---|---|
| Chrome / Edge | 90+ |
| Firefox | 90+ |
| Safari (macOS / iOS) | 15+ |
| Chrome Android | recent |

## 🔒 Privacy

- **No network requests** for your data — everything happens in the browser
- **Two CDN dependencies** loaded on first open: Chart.js and JSZip (these don't see your data — they're libraries, not services)
- **No cookies, no localStorage, no analytics, no tracking**
- **Open source** — read the code and verify yourself

## 🧰 Tech Stack

- Vanilla JavaScript (no framework, no build step)
- [Chart.js 4](https://www.chartjs.org/) for visualizations
- [JSZip 3](https://stuk.github.io/jszip/) for streaming ZIP extraction
- Native `File.stream()` and `TextDecoder` APIs

## 🤝 Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

The most useful contributions:
- Adding new Apple Health metric types
- Improving mobile UX
- Translating the UI to additional languages
- Bug fixes for edge cases in the XML parser

## Disclaimer

Apple, Apple Health, and HealthKit are trademarks of Apple Inc., registered in the U.S. and other countries. This project is not affiliated with, endorsed by, or associated with Apple Inc.

This tool is not a medical device. Do not make medical decisions based on this data.

## 📜 License

[MIT](LICENSE) — do whatever you want, no warranty.

## ⚠️ Disclaimer

This is an analysis tool, not a medical device. Visualizations are based on data your devices recorded; accuracy depends on the sensors. Don't make medical decisions based on this dashboard alone — talk to your doctor.

## 🙏 Acknowledgments

Built with [Chart.js](https://www.chartjs.org/) and [JSZip](https://stuk.github.io/jszip/). Inspired by the frustration of trying to look at three years of heart rate data in the iPhone Health app.
