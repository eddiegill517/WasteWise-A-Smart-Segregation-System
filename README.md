# ♻️ WasteWise: A Smart Waste Segregation Bin

> **Automated, real-time waste classification and sorting using YOLOv5, Raspberry Pi 4, and servo-driven mechanical actuation.**  
> B.E. Capstone Project — Electronics & Communication Engineering, Thapar Institute of Engineering & Technology (TIET), Patiala | December 2024

---

## 📊 Performance Metrics

| Metric | Score |
|---|---|
| **Precision** | 89.8% |
| **Recall** | 82.8% |
| **mAP (Mean Average Precision)** | 82.3% |
| **Dataset Size** | 802 annotated images |
| **Training Epochs** | 300 |
| **Inference Speed** | ~27–30 FPS (live) |
| **Waste Categories** | 3 (Recyclable / Non-Recyclable / Organic) |

---

## 🧠 What is WasteWise?

WasteWise is a fully autonomous smart waste segregation system that classifies and physically sorts waste in real-time — no human intervention required. A user drops waste into the bin; a camera captures the item; a YOLOv5 model classifies it; and servo motors direct it into the correct compartment (Recyclable, Non-Recyclable, or Organic).

The entire pipeline — from image capture to mechanical actuation — runs on a single **Raspberry Pi 4 Model B (8GB)**, making it cost-effective, compact, and deployable in households, college campuses, offices, and public spaces.

---
## System Architecture

**
┌─────────────────────────────────────────────────────────┐
│                        WASTEWISE BIN                    │ 
│                                                         │
│  [WASTE INPUT]  ──►  [DC SLIDING MOTOR]                 │
│                           │                             │
│                    [Pi CAMERA MODULE]                   │
│                           │                             │
│                   [RASPBERRY PI 4 8GB]                  │
│                    ┌──────┴──────┐                      │
│              [YOLOv5 Model]  [OpenCV]                   │
│                    └──────┬──────┘                      │
│                           │ Classification Result       │
│               ┌───────────┼───────────┐                 │
│          [RECYCLABLE]  [NON-REC.]  [ORGANIC]            │
│          Servo 2,3      Servo 1,4   Servo 4             │
│               │              │          │               │
│         [COMPARTMENT]  [COMPARTMENT]  [COMPARTMENT]     │
└─────────────────────────────────────────────────────────┘
**
---

## ⚙️ Hardware Components

| Component | Role |
|---|---|
| **Raspberry Pi 4 Model B (8GB)** | Central processor — runs ML inference, controls all actuators |
| **Raspberry Pi Camera Module** | Real-time image capture of waste items |
| **DC Sliding Motor** | Holds waste in position during classification; releases after |
| **Servo Motor × 4 (Hinge Mechanism)** | Directs waste into the correct compartment post-classification |
| **Power Bank** | Powers all components — Raspberry Pi, DC motor, servo array |
| **Outer Wooden Structure** | Durable housing with three labeled compartments and detachable side logs |

---

## 💻 Software Stack

| Tool | Purpose |
|---|---|
| **YOLOv5** (CSPDarknet53 + PANet) | Real-time object detection and waste classification |
| **Roboflow** | Dataset annotation, augmentation, and model training platform |
| **TensorFlow** | ML model development and deployment framework |
| **OpenCV** | Image pre-processing, feature extraction, video stream handling |
| **Python** | Primary language — inference pipeline, motor control, threading |
| **Raspberry Pi OS** | Lightweight OS optimized for hardware-software integration |

---

## 🔄 How It Works — Step by Step

**Step 1 — Waste Input**  
Waste is placed in the input slot. The DC sliding motor moves forward to hold the item in frame.

**Step 2 — Image Capture & Classification**  
The Pi Camera captures a frame. It is resized to 416×416 and sent to the Roboflow-hosted YOLOv5 model via API. The model returns a bounding box and class label: `Recyclable`, `Non-Recyclable`, or `Organic`.

**Step 3 — Sorting**  
The DC motor releases the item. Based on the classification result, the corresponding servo motors activate their hinge flaps:
- **Recyclable** → Servo 2 & 3 open
- **Non-Recyclable** → Servo 1 & 4 open  
- **Organic** → Default (bottom compartment)

**Step 4 — Bin Maintenance**  
Each compartment has a detachable side log for convenient emptying without disturbing other sections.

---

## 📁 Dataset

- **Total Images:** 802 (custom-captured + augmented)
- **Annotation Tool:** Roboflow
- **Classes:** `Recyclable`, `Non-Recyclable`, `Organic`
- **Split:** 70% Train (564) / 20% Validation (158) / 10% Test (80)
- **Data Collection:** Images captured on TIET campus premises to ensure domain relevance; existing open-source datasets supplemented and expanded with real-world photos


---

## 📈 Training Details

- **Algorithm:** YOLOv5 (You Only Look Once v5) with CSPDarknet53 backbone and PANet feature fusion
- **Platform:** Roboflow Train
- **Epochs:** 300
- **Loss Functions Tracked:** Box Loss (bounding box regression), Class Loss (classification), Object Loss (multi-task combined)
- **Convergence:** All three loss curves showed consistent downward trends across 300 epochs, indicating stable learning

- **mAP Curve:** Model reached ~0.40–0.45 mAP by epoch ~200, stabilizing with minor variance through epoch 300.

---

## 🚀 Getting Started

### 🚀 Prerequisites

```bash
# On Raspberry Pi 4 running Raspberry Pi OS
pip install opencv-python requests numpy gpiozero
pip install inference-cli  # Roboflow inference
```

---

## 🔬 Research Context

**Problem Addressed:** Manual waste sorting is labor-intensive, error-prone, and poses health risks to workers. Most municipal waste — even when partially sorted — ends up contaminated in landfills.

**Key Design Decision:** Unlike prior work using binary dry/wet classification or complex per-material sorting, WasteWise adopts a pragmatic three-class taxonomy (Recyclable / Non-Recyclable / Organic) that balances sorting precision with real-world usability.

**Hardware Optimization:** Prior literature used Raspberry Pi 3 + Arduino combinations. WasteWise consolidates everything onto a single Raspberry Pi 4 (8GB), eliminating integration overhead and reducing cost.

**Known Limitation:** Mixed-material waste items (e.g., a composite packaging with plastic and paper) remain a challenge for single-label classification — flagged as future work.

---

## 🔭 Future Work

- **Mixed Waste Handling** — Improve model robustness for composite/mixed-material items
- **Expanded Categories** — Add e-waste, hazardous waste, medical waste classes
- **Advanced Models** — Explore YOLOv7/v8 or transformer-based vision architectures
- **IoT Integration** — Real-time bin-level monitoring, automated full-bin alerts via cloud/app
- **Energy Efficiency** — Solar-powered variant for off-grid or remote deployments
- **Robotic Arm** — For larger/bulky item handling in industrial-scale versions
- **Mobile Dashboard** — Web/app UI for waste volume analytics and collection scheduling

---

## 👥 Team

| Name | Roll No. | Key Contributions |
|---|---|---|
| **Yuvraj Gill** | 102106007 | Software model development, Roboflow integration, Raspberry Pi integration, system testing |
| Ananya Goel | 102106137 | ML model development & refinement, Roboflow training, integration, testing |
| Shehjaar Manwati | 102106006 | Data collection & preprocessing, model refinement, integration, testing |
| Sheen Sarup | 102106211 | Data collection, hardware (servo + DC motor installation), integration, testing |
| Sartaj Singh | 102106223 | Data collection, hardware construction (wooden structure), motor setup, integration |

**Supervisors:** Dr. Debabrata Ghosh & Dr. Shashikant (Assistant Professors, ECED, TIET)

---

## 📄 Report

The full capstone panel report is available in my repository above.
It covers literature survey, problem formulation, system architecture, training methodology, experimental results, and future directions.

---

## 📜 License

This project was developed as part of the B.E. degree program at Thapar Institute of Engineering & Technology, Patiala. Industrial use and reference welcome with attribution.

---

## 📬 Contact

**Yuvraj Gill** — [github.com/eddiegill517](https://github.com/eddiegill517)

---

**WasteWise bridges the gap between technological potential and practical, accessible waste management solutions.**

