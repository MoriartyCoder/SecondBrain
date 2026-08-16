---
tags:
  - roboter
  - ros2
  - esp32
  - cpp
  - projektplan
date: 2026-08-15
status: in_progress
---

# 🤖 Entwicklungsfahrplan: Sehfähiger Balancier-Roboter

## 📌 System-Architektur (Ziel-Zustand)
- **Laptop (High-Level / C++):** ROS 2 Nodes (OpenCV Vision, BehaviorTree.CPP, robot_localization)
- **Verbindung:** Micro-ROS über Wi-Fi (UDP / DDS)
- **ESP32 (Low-Level / FreeRTOS):** Real-Time Control Loop (100 Hz), Sensor-Fusion (200 Hz), Hardware PCNT Encoder

---

## 🚩 Phase 0: System-Architektur & Entwicklungsumgebung

- [ ] **0.1 Software-Stack aufsetzen**
  - [x] Ubuntu (22.04 LTS / 24.04 LTS) installieren / einrichten
  - [x] ROS 2 (Humble / Iron / Jazzy) installieren (Jazzy)
  - [x] VS Code mit ROS-Extension & C/C++ Tools einrichten
  - [ ] PlatformIO Extension für ESP32-Entwicklung installieren
  - [ ] ROS 2 Pakete installieren:
    - [ ] `ros-<distro>-gazebo-ros-pkgs`
    - [ ] `ros-<distro>-robot-localization`
    - [x] `ros-<distro>-behavior-tree-cpp-v3`
    - [ ] `ros-<distro>-image-transport`
    - [ ] `ros-<distro>-cv-bridge`

- [ ] **0.2 ROS 2 Workspace & Interfaces definieren**
  - [ ] ROS 2 Workspace anlegen (`mkdir -p ~/ros2_ws/src`)
  - [ ] Custom Message / Interface Package erstellen
  - [ ] Interface `/cmd_vel` (`geometry_msgs/msg/Twist`) festlegen
  - [ ] Interface `/imu/data_raw` (`sensor_msgs/msg/Imu`) festlegen
  - [ ] Interface `/target_pose` (`geometry_msgs/msg/PoseStamped`) festlegen

---

## 💻 Phase 1: Simulation-First in Gazebo ("Digital Twin")

- [ ] **1.1 URDF / Xacro Modellierung**
  - [ ] `robot.urdf.xacro` anlegen
  - [ ] Base Link (Chassis, Masse $m_b$, Trägheitsmoment $I_y$, Schwerpunkt $com_z$) definieren
  - [ ] Wheel Links (Räder mit Radius $r$, Abstand $L$) definieren
  - [ ] Continuous Joints für Räder definieren
  - [ ] Gazebo IMU Plugin (`libgazebo_ros_imu_sensor.so`) integrieren
  - [ ] Gazebo Joint State Publisher Plugin einbinden

- [ ] **1.2 C++ Regelungs-Node in ROS 2**
  - [ ] Zustandsraum-Modell / LQR-Matrix $K = [k_1, k_2, k_3, k_4]$ aufstellen
  - [ ] ROS 2 C++ Node schreiben (Abonniert `/imu`, `/joint_states`, `/cmd_vel`)
  - [ ] Publisher für Motor-Effort (`EffortJointInterface`) implementieren
  - [ ] Roboter in Gazebo erfolgreich über Tastatur steuern, ohne dass er umfällt

---

## 🔌 Phase 2: Hardware-Aufbau & Embedded Firmware (ESP32)

- [ ] **2.1 Hardware-Verkabelung & Mechanik**
  - [ ] Chassis montieren (Schwerpunkt möglichst hoch ansetzen)
  - [ ] MPU6050 / BNO055 über I2C mit ESP32 verbinden (Kurzleitungen)
  - [ ] Quadratur-Encoder an GPIOs mit Hardware-Interrupts anschließen
  - [ ] TB6612FNG Treiber verdrahten (PWM LEDC, Direction GPIOs, STBY an HIGH)
  - [ ] 3S LiPo Stromversorgung und Spannungsregler absichern

- [ ] **2.2 ESP32 C++ Firmware-Architektur (FreeRTOS)**
  - [ ] PlatformIO-Projekt für ESP32 initialisieren
  - [ ] **Task 1 (Sensor Fusion @200 Hz, Core 1):** Komplementärfilter / Kalman-Filter schreiben
  - [ ] **Task 2 (Encoder Readout @100 Hz, Core 1):** ESP32 PCNT Hardware-Modul konfigurieren
  - [ ] **Task 3 (Control Loop @100 Hz, Core 1):** LQR/PID Regler aus Phase 1 auf ESP32 portieren
  - [ ] **Sicherheits-Not-Aus:** Motoren stoppen, falls $|\theta| > 35^\circ$
  - [ ] Stand-Alone Hardware-Test: Roboter gleicht Stöße per Hand autonom aus

---

## 📡 Phase 3: Micro-ROS Integration & Sensor Fusion

- [ ] **3.1 Micro-ROS auf dem ESP32**
  - [ ] `micro_ros_platformio` Bibliothek einbinden
  - [ ] **Task 4 (Core 0):** UDP Transport über Wi-Fi konfigurieren
  - [ ] Publisher für `/imu/data_raw` und `/wheel_odometry` erstellen
  - [ ] Subscriber für `/cmd_vel` erstellen (Offset-Aufschaltung auf Regler)

- [ ] **3.2 High-Level Sensor Fusion auf dem Laptop**
  - [ ] Micro-ROS Agent starten (`micro_ros_agent udp4 --port 8888`)
  - [ ] `robot_localization` EKF-Node aufsetzen:
    - [ ] Input 1: IMU-Daten vom ESP32
    - [ ] Input 2: Wheel-Odometry vom ESP32
  - [ ] Verifizieren, dass `/odometry/filtered` saubere Raum-Positionen liefert

---

## 👁️ Phase 4: Computer Vision Pipeline (OpenCV C++)

- [ ] **4.1 Kamera-Kalibrierung**
  - [ ] Schachbrett-Muster ausdrucken
  - [ ] Calibration über ROS 2 Paket `camera_calibration` durchführen
  - [ ] Intrinsic Matrix $K$ und Verzeichnung $D$ speichern

- [ ] **4.2 C++ Computer Vision Node**
  - [ ] ROS 2 C++ Node erstellen, der `/image_raw` abonniert
  - [ ] Bild-Konvertierung via `cv_bridge::toCvCopy` einrichten
  - [ ] ArUco-Marker / Objekt-Erkennung via OpenCV implementieren
  - [ ] Relativer 3D-Pose-Estimator (`cv::aruco::estimatePoseSingleMarkers`) aufsetzen
  - [ ] Zielposition auf Topic `/target_pose` veröffentlichen

---

## 🧠 Phase 5: Decision Making & Autonomie (BehaviorTree.CPP)

- [ ] **5.1 Verhaltensbaum XML erstellen**
  - [ ] `tree.xml` definieren (Fall Check, Target Search, Target Tracking)

- [ ] **5.2 C++ Nodes für BehaviorTree.CPP**
  - [ ] Custom Condition Node: `IsRobotFallen`
  - [ ] Custom Condition Node: `IsTargetVisible`
  - [ ] Custom Action Node: `TrackTarget` (P-Regler auf Ziel-Distanz -> `/cmd_vel`)
  - [ ] Custom Action Node: `SpinInPlace` (Suchmodus)
  - [ ] Haupt-ROS 2 Node schreiben, der den Verhaltensbaum lädt und ausführt
  - [ ] Systemtest: Roboter sucht Marker, fährt darauf zu und hält bei 20cm Abstand an

---

## ⏱️ Phase 6: Performance-Optimierung & Benchmarking

- [ ] **6.1 Latenz-Analyse**
  - [ ] Ende-zu-Ende-Latenz messen (Kamera -> Laptop -> Wi-Fi -> ESP32 -> Motor)
  - [ ] Micro-ROS QoS-Profile auf *Best Effort* zur Latenzreduzierung umstellen

- [ ] **6.2 Robustheits-Tests**
  - [ ] Sprungantwort bei Zusatzgewicht testen (LQR-Kompensation prüfen)
  - [ ] Dynamic Obstacle Test im Kamerabild durchführen