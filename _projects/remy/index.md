---
layout: post
title: Remy — 2-Axis Animatronic Head
description: An evolving wearable animatronic head, from a tethered Teensy prototype to a compact, battery-powered ESP32-C3 build with live IMU head tracking.
main-video: Remy.mp4
skills:
  - Embedded C++
  - Teensy 3.6
  - ESP32-C3
  - MPU-6050 / GY-521 IMU
  - I2C
  - Hardware PWM
  - Sensor Fusion
  - Power Electronics
  - Servo Control
  - 3D Printing
---

## Project Overview

Remy is a compact, 2-axis animatronic head driven by two 9g micro servos. The project evolved through two major iterations: a tethered v1 prototype with randomized motion, followed by a smaller, battery-powered v2 design with real-time head tracking using a 6-axis accelerometer/gyroscope.

## v1 — Initial Prototype

The first version used a Teensy 3.6, two 9g micro servos, and a USB phone-charger power brick. It established the mechanical concept and demonstrated automated head movement.

{% include local-video.html src="/_projects/remy/v1lookaround.mp4" size="large" caption="v1 lookaround — the original prototype uses randomized servo movements to look from side to side." %}

### Power Limitations and Scope Changes

In v1, both servos were powered directly from the Teensy's onboard power pin. Servo startup and simultaneous movement caused voltage drops, creating a risk of brownouts. With limited time before returning to classes, I used a software workaround rather than redesigning the power distribution: I alternated servo movements so both motors would not draw peak current at the same time.

I also explored live motion input using an accelerometer and tested a wired joystick. The accelerometer integration was not reliable enough to finish within the available time, and the joystick wiring felt restrictive. I cut live input and used an automated sequence of randomized head movements instead.

**v1 outcome:** a working proof of concept for the mechanical linkages, but one that remained tethered to external power and could not respond to real-time head movement.

## v2 — Compact, Battery-Powered Redesign

The second version focuses on reducing the physical footprint, moving to untethered battery power, and implementing reliable real-time head tracking.

**Hardware**
- Seeed Studio XIAO ESP32-C3
- GY-521 (MPU-6050) 6-axis IMU
- Two 9g micro servos
- 1S LiPo battery
- 5V boost converter
- 2.54mm double-sided perfboard

### 1. Redesigning Power Distribution

The v1 power arrangement routed servo current through the microcontroller's onboard power pin. This caused voltage sag when the servos moved and would be especially problematic for the ESP32-C3.

For v2, I added a dedicated 5V boost converter powered by a 1S LiPo battery and verified its output under load with a multimeter. The 5V and ground rails are wired in parallel to the servos, IMU, and XIAO 5V input, with a shared common ground and decoupling capacitance to help absorb servo current spikes.

### 2. Debugging I2C Communication

The GY-521 initially failed to appear reliably on the XIAO's I2C bus. I isolated the issue by testing the sensor on a known-good Arduino Uno, confirming its 0x68 address and raw output, inspecting and reflowing weak header solder joints, and bypassing unreliable breadboard contacts.

I remapped the XIAO ESP32-C3's I2C pins to D8 (GPIO 8) and D9 (GPIO 9) using `Wire.begin(8, 9)`. I also caught board-target mismatches in the IDE when switching between AVR and ESP32 architectures.

### 3. Eliminating Servo Jitter with Sensor Fusion

Raw accelerometer readings picked up small vibrations that made the servos buzz and twitch. The standard AVR servo-library approach also did not map cleanly to the ESP32-C3.

I implemented a custom 50 Hz PWM driver using the ESP32's native `ledc` hardware timers on D0 and D1. For smoother orientation estimates, I added a complementary filter that blends 96% gyroscope rate data with 4% accelerometer tilt data. This reduced servo jitter while correcting the long-term drift associated with gyroscope-only measurements.

### IMU Demonstration

Getting the accelerometer/gyroscope tracking working was a major debugging milestone, so I included a separate demonstration of the sensor-driven movement.

{% include local-video.html src="/_projects/remy/accelerometerworks.mp4" %}

### 4. Compact Packaging and Battery Connections

I moved from a sprawling breadboard layout to a compact 2.54mm double-sided perfboard. Female socket headers keep the XIAO removable for reprogramming, while a dedicated power switch and keyed 2-pin JST connectors make the battery easier to disconnect for charging. Staggered, heat-shrunk wire splices help protect the stranded battery leads from fraying and shorting.

## Engineering Takeaways

- **Separate logic power from motor power:** A dedicated 5V rail with adequate current capacity and decoupling addresses voltage sag at the hardware level instead of relying on movement timing as a workaround.
- **Isolate hardware from firmware while debugging:** Testing the IMU on a known-good board helped distinguish a sensor problem from pin-mapping, soldering, and breadboard-connection issues.
- **Mechanical and electrical reliability go together:** Moving from temporary breadboard wiring to soldered perfboard, removable headers, and keyed connectors made the compact build more practical to use and maintain.

## Remy on Head

{% include local-video.html src="/_projects/remy/Remy_On_Head.mp4" %}

## Acknowledgments

The mechanical concept and original STL model were inspired by [She Builds Robots' Remy Model](https://github.com/shebuildsrobots/remy_model). The idea of using an accelerometer to detect head orientation and drive interactive movement also came from the original creator. My implementation differs: I moved from the original controller to a Teensy 3.6 for v1, and then to a Seeed Studio XIAO ESP32-C3 for v2, requiring a different sensor integration and control implementation.
