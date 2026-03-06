# ✈️ AI-Based Airport Security Surveillance System

An intelligent **Computer Vision-based Airport Surveillance System** built using **YOLOv8**, multi-object tracking, crowd density analysis, optical flow, and abandoned luggage detection.

This project processes surveillance video to:

- 👤 Detect and track people
- 🧳 Detect and monitor luggage
- 📊 Analyze crowd density
- 🚨 Detect overcrowding zones
- 🎒 Identify abandoned luggage
- 🌊 Analyze crowd movement using optical flow
- 🎥 Generate an annotated output video with alerts

---

# 📌 Features

## 1️⃣ Person & Luggage Detection
- Powered by **YOLOv8 (Ultralytics)**
- Detects:
  - Person (class 0)
  - Backpack
  - Suitcase
  - Handbag

## 2️⃣ Multi-Object Tracking
- ByteTrack-style custom tracker
- Assigns unique IDs to persons
- Maintains trajectories
- Handles lost/reappearing objects

## 3️⃣ Crowd Density Analysis
- Divides frame into grid (default: 5x5)
- Calculates persons per square meter
- Detects overcrowded zones
- Generates heatmap overlay

## 4️⃣ Optical Flow Analysis
- Dense Farneback Optical Flow
- Detects:
  - Bottlenecks
  - Panic-level movement

## 5️⃣ Abandoned Luggage Detection
- Associates luggage with nearest person
- Tracks distance & time alone
- Raises alert if luggage remains unattended
- Displays flashing red warning

## 6️⃣ Smart Video Output
- Bounding boxes
- Person IDs
- Trajectories
- Overcrowding alerts
- Abandoned luggage alerts
- Real-time statistics overlay

---

# 🛠️ Technologies Used

- Python
- OpenCV
- NumPy
- Ultralytics YOLOv8
- FilterPy
- SciPy
- Matplotlib

---

 

