---
layout: single
title: "Wearable Hand Motion Sensing for 3D Trajectory Reconstruction of Hypothenar Rolling"
permalink: /research/
author_profile: true
---

## Background and research question

When students of traditional Chinese medicine practice hypothenar rolling, each back-and-forth movement has its own rhythm, amplitude, and turning points. After a brief visual calibration, can the glove’s sensors alone reconstruct the 3D path of the knuckles and distinguish slower rolls from larger swings? Comparing these measurements with a reference technique could give students a quantitative way to track their progress.

<figure style="width: 74.8%; max-width: 23.76rem; margin-inline: 0;">
  <img src="{{ '/images/shoutao.jpg' | relative_url }}" alt="Prototype of the wearable Tuina glove">
  <figcaption>Glove prototype used to record hypothenar rolling movements.</figcaption>
</figure>

## My contribution

I carried out this project independently. I used an STM32 microcontroller to collect data from two inertial measurement units (IMUs) and a 48-channel pressure array, and built a Unity application to receive, display, and record the data. I tracked an ArUco marker on the hand with stereo cameras and reconstructed the 3D path of its center as a reference. I then compared several time-series models and designed experiments to test whether calibration on the first recording improved trajectory estimates on subsequent recordings after the glove was put on again.

<figure style="width: 74.8%; max-width: 29.92rem; margin-inline: 0;">
  <img src="{{ '/images/unity.jpg' | relative_url }}" alt="Unity acquisition interface showing readings from both IMUs and a contact pressure heatmap" style="object-view-box: inset(23.5% 19.3% 14.9% 31.6%);">
  <figcaption>The Unity application displays readings from both IMUs and the contact pressure distribution in real time.</figcaption>
</figure>

## System and evaluation

<figure>
  <img src="{{ '/images/tuina/system-overview.png' | relative_url }}" alt="Training and deployment workflow. Stereo vision supplies a reference for training, and IMU data are used to estimate the 3D hand trajectory during deployment.">
  <figcaption>Stereo vision provides the reference trajectory. After personal calibration, the network estimates the trajectory from the two IMUs alone.</figcaption>
</figure>

I used the stereo reference trajectories for model training, initial personal calibration, and offline scoring. For personal calibration, I used normal-speed rolling from the first recording to tune the output layer of a temporal convolutional network (TCN), keeping the other parameters fixed. The calibrated model then estimated trajectories using only the two IMUs. Stereo data from subsequent recordings were used solely for scoring. I used the 48-channel pressure array to record and analyze the contact pressure distribution.

I measured the difference between the predicted and reference trajectories using 3D root mean square error (RMSE). I applied a single translation to each recording for scoring, preserving the original direction, scale, and timing of the movement.

## Results

I (operator A) put the glove on again and used the first recording from that session, R04, for personal calibration. On the next two recordings, R05 and R06, the model reconstructed the knuckle trajectory using only the two IMUs. **Relative 3D trajectory RMSE was 2.11 mm for normal-speed rolling and 1.87 mm for slow rolling.** Earlier recordings of mine had contributed to training the base model. Both test recordings were newly collected. For each random seed, I averaged the errors across the two test recordings, then reported the median of those averages across three seeds.

In two other recordings from the same session, I varied rolling amplitude and speed separately. Both switches from small to large movements increased the predicted swing amplitude. Both switches from slow to normal-speed rolling shortened the predicted duration of a back-and-forth cycle.

#### X, Y, and Z over time during normal-speed rolling by operator A

<figure>
  <img src="{{ '/images/tuina/r05-xyz.png' | relative_url }}" alt="Stereo reference and dual-IMU predictions for X, Y, and Z during normal-speed rolling in operator A’s R05 recording.">
</figure>

The gray line is the stereo reference, and the blue line is the dual-IMU prediction. Along the X axis, the prediction tracks the peaks and troughs of each cycle with an amplitude close to the reference.

#### Reconstructed 3D trajectory from the same recording

<figure style="display: block; aspect-ratio: 960 / 780; overflow: hidden;">
  <img style="display: block; width: 100%; height: auto; margin: 0;" src="{{ '/images/tuina/r05-3d.png' | relative_url }}" alt="Predicted 3D trajectory after personal calibration and stereo reference during normal-speed rolling in R05.">
</figure>

The blue prediction follows the gray reference back and forth, with local deviations near the turning points at both ends. Both plots show the same normal-speed segment of R05 with the random seed fixed at 100. Applying one translation over the full 5–30 s interval gave a relative 3D trajectory RMSE of **1.89 mm** for this example.

## Limitations and next steps

I have built the system and evaluated it offline. For a different operator, the model can still underestimate the range of motion near the turning points even after personal calibration. Reconstructing trajectory shape across operators and resolving motion along the camera’s depth axis remain open challenges.
