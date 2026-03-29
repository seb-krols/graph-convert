# GraphConvert

> Right-click any graph. Get the data. Feed it to AI.

<!-- HERO: Replace with GIF of right-click → extraction flow -->
![GraphConvert Demo](https://github.com/user-attachments/assets/4390ae51-06e5-4e46-a6b6-d33cdff9fbe0)

[![Chrome Web Store](https://img.shields.io/chrome-web-store/v/YOUR_EXTENSION_ID?label=Chrome%20Web%20Store&logo=google-chrome&logoColor=white&color=4F46E5)](https://chromewebstore.google.com/detail/cepidnjndbmllpkigekbocfjakmfjlko?utm_source=item-share-cb)
[![Chrome Web Store Users](https://img.shields.io/chrome-web-store/users/YOUR_EXTENSION_ID?color=4F46E5)](https://chromewebstore.google.com/detail/cepidnjndbmllpkigekbocfjakmfjlko?utm_source=item-share-cb)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## The Problem

AI tools can't read graphs. Upload a chart to ChatGPT or Claude and the best you'll get is *"this appears to show an upward trend"* — no numbers, no precision, no actual data. Every researcher, engineer, and student who's ever tried to digitize a graph from a paper or textbook knows the pain: you're either squinting and manually writing down values, or you're stuck with vague qualitative observations.

GraphConvert fixes this in seconds.

---

## What It Does

GraphConvert is a Chrome extension that extracts precise (x, y) data points from any graph image on the web. Right-click, calibrate your axes, and get back clean numerical data ready to paste straight into an AI prompt, spreadsheet, or analysis tool.

<!-- POPUP: Replace with screenshot of extension popup -->
![Extension Popup](https://github.com/user-attachments/assets/c735df1c-4b0c-4bb3-b0c8-c979127f4630)

---

## How It Works

1. **Right-click** any graph image on any webpage
2. **Select** "Digitize Graph with GraphConvert"
3. **Calibrate** — enter your axis min/max values (reads directly from the graph)
4. **Extract** — choose 25, 50, or 100 data points
5. **Export** as JSON, CSV, or TXT — ready for AI, Excel, or Python

<!-- LANDING PAGE: Replace with screenshot of landing page -->
![GraphConvert Landing Page](https://github.com/user-attachments/assets/ff493d2d-25b4-44f0-8a66-b780dad709db)

---

## Technical Architecture

GraphConvert runs a full client-server pipeline:

**Chrome Extension (Manifest V3)**
Captures the graph image via context menu, handles axis calibration UI, displays the extracted point overlay on the original image, and manages export.

**FastAPI Backend (Railway)**
Does the heavy lifting — Hough transform axis detection, curve isolation via color filtering and morphology, centroid-based point sampling, and pixel-to-coordinate transformation. Processes images in RAM only, never stored.

**Supabase**
Handles user authentication, usage tracking (server-side enforced, can't be bypassed), and the coupon system.

```
Chrome Extension
      │  HTTPS POST (image + axis values)
      ▼
FastAPI Backend (Railway)
  ├── Axis detection (Hough transform)
  ├── Curve extraction (color filter + morphology)
  ├── Coordinate mapping (pixel ↔ real world)
  └── Auth + usage tracking
      │
      ▼
Supabase (users, usage_logs, coupons)
```

---

## Core Algorithm

The digitization pipeline runs in 6 steps:

1. **Axis detection** — Hough transform finds the X and Y axis lines
2. **Plot area bounds** — axis intersection defines the usable graph region
3. **Manual calibration** — user enters x_min, x_max, y_min, y_max from the graph labels
4. **Curve isolation** — color filtering and morphological operations isolate the curve pixels
5. **Point sampling** — for each sampled x-column, centroid method finds the curve's y-pixel
6. **Coordinate transform** — pixel positions mapped to real-world values via linear interpolation

---

## Tech Stack

| Layer | Tools |
|---|---|
| Extension | Chrome Manifest V3, JavaScript |
| Backend | Python, FastAPI |
| Image Processing | OpenCV, NumPy |
| OCR | Tesseract (pytesseract) |
| Database | Supabase (PostgreSQL) |
| Hosting | Railway |
| Payments | Stripe |

---

## Features

- **Right-click capture** — works on any graph image on any webpage
- **3 output formats** — JSON (AI-ready), CSV (Excel-ready), TXT (universal)
- **25 / 50 / 100 point extraction** — choose your resolution
- **Visual overlay** — extracted points drawn over original image for verification
- **Server-side usage enforcement** — can't be bypassed by clearing browser data
- **Privacy-first** — images processed in RAM only, never saved to disk or database
- **Freemium** — 10 graphs/day free, unlimited on Pro

---

## Pricing

| Plan | Price | Limit |
|---|---|---|
| Free | $0 | 10 graphs/day |
| Pro Monthly | $4/month | Unlimited |
| Lifetime | $15 one-time | Unlimited forever |

---

## Status

**Live on the Chrome Web Store.** Full pipeline working end-to-end.

<!-- STORE: Replace with Chrome Web Store screenshot -->
![Chrome Web Store](https://github.com/user-attachments/assets/4462977d-8ec4-4eea-9cab-64abbd2bd162)

**Roadmap:**
- Auto axis detection via OCR (Pro feature)
- Multi-curve support (color-based detection)
- Log scale support
- Firefox extension

---

## Install

**[→ Add to Chrome](https://chromewebstore.google.com/detail/cepidnjndbmllpkigekbocfjakmfjlko?utm_source=item-share-cb)**

Or load unpacked for development:
```bash
# Clone the repo
git clone https://github.com/seb-krols/graphconvert.git

# Load in Chrome
# chrome://extensions → Enable Developer Mode → Load Unpacked → select extension/
```

---

## Contact

**Sebastian Krols**
[github.com/seb-krols](https://github.com/seb-krols) · [LinkedIn](https://linkedin.com/in/sebastiankrols)

Questions, feedback, or want to collaborate? Feel free to open an issue or reach out directly.

---

*© 2026 Sebastian Krols. All rights reserved.*
