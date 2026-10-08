---
layout: single
title: "Research"
permalink: /research/
author_profile: true
---

## Smart Tuina Glove: Local 3D Trajectory Reconstruction of the Hypothenar Rolling Manipulation

### Background

The rolling manipulation (Ding's rolling method) is a representative technique in Traditional Chinese Medicine (TCM) Tuina. However, traditional teaching relies on hands-on experience passed down from masters. Whether a movement is performed correctly — the forward-rolling angle, applied force, frequency, and whether the contact point stays anchored — lacks objective, quantitative evaluation and cannot be measured with data.

### Method

- **Wearable sensing**: dual MPU6050 inertial sensors (back of hand + wrist) and a 48-channel thin-film pressure array, STM32 MCU, Bluetooth transmission.
- **Host computer**: a Unity-based acquisition host program handling serial communication and binary data parsing, real-time display of the pressure heatmap and hand pose, and synchronized multi-stream data logging. Companion Python scripts perform sensor calibration, stereo visual ground truth, and time synchronization.
- **Visual ground truth**: a stereo camera with ArUco markers provides 3D hand position and pose ground truth via calibration and triangulation (baseline 64.79 mm, static accuracy 0.19 mm).
- **Deep learning**: reconstructs the hand's 3D trajectory from IMU + pressure signals. Training uses the visual ground truth, while deployment only requires the wearable sensors — no camera or motion-capture system needed.

<img src="{{ '/images/unity.jpg' | relative_url }}" alt="Unity host computer" style="width: 75%;">

*Unity host computer interface*

### Contributions

1. Quantifies the correctness of a TCM manipulation into measurable metrics — forward/return rolling angle, force ratio, frequency, and contact-point slip distance.
2. Reconstructs trajectories in a small local region.
3. A wearable solution that fits real teaching and clinical scenarios at a much lower cost than optical motion capture.
4. A dual-IMU design with a clear biomechanical basis (forearm / wrist).

### Current Progress

Completed sensor calibration, stereo camera calibration, accuracy verification (static ≤0.2 mm, displacement ≤1 mm), and camera–IMU time synchronization (<10 ms). Feasibility data collection and network training are in progress.

<img src="{{ '/images/trajectory.jpg' | relative_url }}" alt="Simulated trajectory" style="width: 50%;">

*Simulated visual trajectory reconstruction of the hypothenar rolling*
