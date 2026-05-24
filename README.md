# Multi-Machine Monitoring Solution including FANUC Controllers with License-Free OEE Analytics

---

## Overview

A **hardware-based plug-and-play solution** that monitors multiple CNC machines (FANUC, Mazak, etc.) **without expensive licenses** (no FOCAS required). Uses custom G-code macro variables to extract cycle time, downtime, and production data — then calculates real-time OEE.

---

## How It Works 

1. **Hardware Device** — Raspberry Pi (32-bit OS) connects to CNC via PROFIBUS/Ethernet
2. **G-code Macros** — Custom macro variables (#500, #501, etc.) store real-time operational data inside the CNC controller
3. **Python Script** — Reads macro values every second 
4. **Node-RED Dashboard** — Visualizes OEE (Availability × Performance × Quality)
5. **Cloud Database** — Historical analysis & trend tracking

---

## 📊 OEE Metrics Captured

| Metric | Macro Variable | Method |
|--------|---------------|--------|
| Cycle Start Time | #500 | Captured at program start |
| Cycle End Time | #501 | Captured at M30 |
| Run Time | #503 | Incremented every second |
| Downtime Flag | #504 | 0=Running, 1=Idle, 2=Down |
| Parts Produced | #505 | Per cycle counter |
| Rejected Parts | #506 | Quality inspection input |
| Spindle Load | #508 | Real-time load monitoring |

---

## 🚀 Quick Start (For Implementation Partners)

1. **Hardware:** Raspberry Pi 3/4
2. **CNC Setup:** Load G-code macros 
3. **Connect:** Pi to CNC via Ethernet 
4. **Deploy:** Run the Node-RED dashboard
5. **Monitor:** Access dashboard at `http://localhost:1880`

---

## 📈 Results

| Before | After |
|--------|-------|
| Manual data logging | Real-time automated monitoring |
| Proprietary license costs | License-free solution |
| No OEE visibility | Live OEE dashboard |

---

