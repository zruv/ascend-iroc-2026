# ASCEND: Autonomous Systematic Coverage and Exploration in No-GPS Environments

**Team Drone Sentience (TeamID-10570)**

---

## 🚀 Project Overview
The ASCEND project is a specialized robotic solution engineered for the ISRO Robotics Challenge - URSC. It is a fully autonomous quadcopter designed to conduct systematic exploration, feature detection, and precise localization in environments where GPS signals are unavailable or unreliable.

> **Notice regarding competitive integrity:** This repository serves strictly as a design, hardware, and media showcase. Proprietary autonomy scripts, heuristics algorithms, SIFT validation pipelines, and specific EKF3 parameter configurations have been omitted to protect the core IP of the project.

![Arcaruco marker](/assests/tasks%20overview.png)

---

## 🧠 System Architecture
The system architecture utilizes a robust master-slave embedded computing paradigm to guarantee real-time flight stability while managing heavy cognitive workloads.

* **Avionics Layer (Slave):** A Pixhawk 2.4.8 flight controller running ArduPilot firmware handles low-level, high-frequency flight stabilization, motor mixing, and safety-critical failsafes independently [cite: 1, 2].
* **Companion Computer Layer (Master):** An NVIDIA Jetson Orin Nano Super running Ubuntu 22.04 with ROS2 serves as the cognitive core. It processes visual data and issues precise velocity setpoints and waypoint commands to the avionics layer over MAVLink.

---

## 🚁 Hardware & Sensor Specifications
The platform has been meticulously upgraded to balance payload capacity, agility, and rigorous operational demands.

### Core Airframe & Propulsion
* **Airframe:** S500 quadcopter frame, selected for its sturdiness and expanded capacity to house multiple components.
* **Propulsion:** EMAX MT2213 935KV brushless motors driven by Favorite LittleBee 30A ESCs (supporting BLHeli-S firmware for superior control).
* **Power Source:** Pro-range 14.8V 4500mAh 35C 4S Li-Po Battery Pack targeting optimized flight time and endurance.

### Perception & Odometry Suite
* **Primary Vision Sensor:** SJCAM SJ4000 operating in USB webcam mode for wide-FOV visual input.
* **Optical Flow & Altitude:** Mico Air MTF-01 provides horizontal distance data for obstacle avoidance and velocity estimation.

---

## 🛰️ Core Capabilities

### 1. GPS-Denied Autonomous Navigation
ASCEND navigates entirely without external GPS by fusing data from its onboard IMU, optical flow, and downward-facing camera via Visual-Inertial Odometry (VIO). The exploration phase traverses the arena using a structured boustrophedon (lawnmower) pattern. The drone utilizes a custom heuristics-based approach—using cell indices and spatial equations—to determine precise navigation coordinates while minimizing computational overhead.

### 2. Autonomous Survey & Feature Validation
During the lawnmower survey, the drone captures HD imagery (1280x720). The images are tagged with cell-index metadata. Once transmitted to the base station via Wi-Fi, the data is down-sampled and processed using the Scale-Invariant Feature Transform (SIFT) algorithm. SIFT rapidly extracts local feature vectors and compares them against reference images, offering high robustness against variations in scale and rotation.

### 3. Precision Docking & Autonomous Charging
* **Visual Landing:** The drone utilizes a 6x6 ArUco fiducial marker (100mm, ID=0) centered on the base station platform to execute a pinpoint touchdown, dynamically adjusting its trajectory to prevent skidding.
* **Autonomous Charging:** Upon landing, contact pads mate with the drone's sheet metal battery leads. A CV/CC buck-boost module automatically replenishes the 4S LiPo battery, configured to 16.8V and a 1A current limit. Real-time status is monitored via an INA219 sensor, alongside a multi-layered safety firmware incorporating thermal cutoff triggers and short-circuit protection.

---

## 🛡️ Failsafe & Mitigation Protocols
The architecture includes a layered safety net to handle unpredictable environments:
* **Low Battery Protocol:** ArduPilot parameters continuously monitor voltage, triggering an automatic Land mode on critical thresholds.
* **Signal & Thrust Loss:** The system features an immediate transition to a controlled Land mode if the RC signal is lost or if propulsion thrust falls below safe operational limits.
* **Thermal Throttling Mitigation:** An active 5V brushless cooling fan and optimized ROS2 workload scheduling on the Jetson prevent computational bottlenecks during sustained operations.

---

### High-Level Simulations
![Hardware-Software Interaction Map](/assests/hardware-software%20interaction%20map.png)

<video width="100%" autoplay loop muted controls>
  <source src="/assets/aruco marker.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>