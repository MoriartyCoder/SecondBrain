---
tags:
  - perception
  - computer_vision
  - ros2
  - tensorrt
  - cpp
  - autonomous_systems
date: 2026-08-15
status: in_progress
---

# 👁️ Entwicklungsfahrplan: Perception & Vision für Autonome Systeme

## 🚩 Stufe 0: Grundlagen & Dev-Environment
- [ ] **0.1 C++ & CUDA Grundlagen**
  - [ ] Eigen3 (Linear Algebra) in C++ integrieren
  - [ ] OpenCV C++ API konfigurieren
  - [ ] CUDA Basic Memory Management & Kernel Concept verstehen
- [ ] **0.2 Tools & Visualization Setup**
  - [ ] ROS 2 (Humble/Jazzy) & Foxglove Studio installieren
  - [ ] Benchmarking-Tools für GPU/CPU-Latenz aufsetzen (TensorRT profiler)

---

## 🚗 Stufe 1: High-Speed 2D Detection & Tracking Pipeline (C++)
- [ ] **1.1 PyTorch -> TensorRT Export**
  - [ ] Ultralytics YOLOv8/v10 Modell in PyTorch wählen
  - [ ] Modell nach `.onnx` exportieren
  - [ ] Engine-Datei mit `trtexec` für FP16/INT8 Quantisierung kompilieren
- [ ] **1.2 Real-time C++ Inference Engine**
  - [ ] TensorRT C++ API Wrapper schreiben (Memory Allocation, Host-to-Device Copy)
  - [ ] NMS (Non-Maximum Suppression) in C++ / CUDA implementieren
  - [ ] OpenCV VideoStream an TensorRT Engine anbinden (> 60 FPS Ziel)
- [ ] **1.3 Multi-Object Tracking (MOT)**
  - [ ] ByteTrack oder SORT Algorithmus in C++ einbinden
  - [ ] Eindeutige Object IDs über Frames hinweg nachverfolgen
  - [ ] Geschwindigkeits- und Richtungsvektoren der Objekte schätzen

---

## 📐 Stufe 2: 3D Spatial Vision & Visual SLAM
- [ ] **2.1 Kamera-Geometrie & Tiefenschätzung**
  - [ ] Intrinsische ($K$) & Extrinsische ($R, T$) Kameramatrizen verstehen
  - [ ] Monokulare Tiefenschätzung (z. B. Depth Anything) oder Stereo-Disparität einbinden
  - [ ] Generierung von Point Clouds aus RGB-D / Stereo-Bildern in C++
- [ ] **2.2 3D Bounding Box Estimation**
  - [ ] 2D Bounding Boxes mit Tiefenkarte kombinieren -> 3D Bounding Box $(X, Y, Z, W, H, L, Yaw)$
  - [ ] Visualisierung der 3D Bounding Boxes im 3D-Raum (Foxglove Studio / Open3D)
- [ ] **2.3 Visual SLAM / Odometry**
  - [ ] ORB-SLAM3 oder RTAB-Map ROS 2 Node aufbauen
  - [ ] Feature Extraction & Matching (ORB/FAST) nachvollziehen
  - [ ] Eigenbewegung der Kamera (Visual Odometry) ohne Wheel-Encoder schätzen

---

## 🌐 Stufe 3: Sensor Fusion & Bird's Eye View (BEV)
- [ ] **3.1 Dataset-Arbeit (nuScenes / KITTI)**
  - [ ] nuScenes devkit in Python einrichten
  - [ ] Extraktion synchronisierter Multi-Kamera- und LiDAR-Daten
- [ ] **3.2 LiDAR-Camera Projection (Sensor Fusion)**
  - [ ] Kalibrierungs-Transformation zwischen Kamera- und LiDAR-Koordinatensystem
  - [ ] Projektion von 3D-LiDAR-Punktwolken auf 2D-Kamerabilder
  - [ ] Sensor Fusion mit Erweitertem Kalman Filter (EKF) oder Graph-Optimization
- [ ] **3.3 Bird's Eye View (BEV) Mapping**
  - [ ] LSS (*Lift, Splat, Shoot*) Konzept oder BEV-Transformer (z. B. BEVFusion) verstehen
  - [ ] Projektion von Multi-Kamera-Bildern in ein 2D-Gitter der Vogelperspektive
  - [ ] Map-Generation: Drivable Area Grid & Dynamic Obstacle Map

---

## ⚙️ Stufe 4: End-to-End ROS 2 Perception Pipeline
- [ ] **4.1 ROS 2 Node Architecture**
  - [ ] Node 1: `image_publisher` / Sensor Interface
  - [ ] Node 2: `tensorrt_detector_node` (Publiziert `/perception/tracked_objects`)
  - [ ] Node 3: `bev_grid_generator_node` (Publiziert `/perception/occupancy_grid`)
- [ ] **4.2 Performance Tuning**
  - [ ] Zero-Copy Memory Transport mit ROS 2 intra-process communication aktivieren
  - [ ] Latenzmessung: Bildaufnahme -> Perception Output (< 30 ms)