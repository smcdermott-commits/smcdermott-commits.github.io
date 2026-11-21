---
layout: post
title: Laser Tripwire Alarm System
order: 2
main-video: tripwire.mp4
description: An Arduino-based break-beam alarm using a KY-008 red laser transmitter and KY-033 optical sensor, with PWM-dimmed green status and red alarm indicators.
skills:
  - Arduino Uno
  - Embedded C++
  - KY-008 Laser Transmitter
  - KY-033 Optical Sensor
  - Digital I/O
  - PWM
  - Breadboard Prototyping
---

## Project Overview

This exploratory build was an opportunity to get hands-on experience with optical sensors, breadboard prototyping, and digital and PWM signal control on an Arduino Uno. Starting with a kit of unfamiliar sensor modules, I paired a KY-008 red laser transmitter with a KY-033 infrared line-tracking sensor to create a break-beam tripwire alarm with visual status indicators.

## Live Demonstration

Here’s a walkthrough of the tripwire alarm in action, including a demonstration of the sensor and alarm response.

{% include local-video.html src="/_projects/laser-tripwire/tripwire.mp4" size="large" caption="Demonstrating the laser tripwire alarm system." %}

## How It Works

The KY-008 projects a continuous red laser beam toward the optical receiver on the KY-033. The sensor output controls two status LEDs:

- **Beam intact — secure:** The receiver detects the beam and holds its output `LOW`. The Arduino keeps a dimmed green indicator illuminated using PWM.
- **Beam broken — alarm:** When an object interrupts the beam, the sensor output switches `HIGH`. The green indicator turns off and a bright red LED signals that the tripwire has been triggered.

## Build Process & Troubleshooting

### Repurposing and Calibrating the KY-033

I first tested the KY-033 as a standalone reflective line-tracking sensor, observing how it responds differently to light surfaces and dark, IR-absorbing surfaces. I then adjusted its onboard potentiometer to make the receiver respond reliably to the focused laser across an open gap. This involved learning how the module's sensitivity threshold affects its digital output.

### Maintaining Optical Alignment

The small sensor boards shifted easily when pulled by jumper wires, making the laser-to-receiver alignment inconsistent. I switched to female-to-male DuPont leads and anchored the modules so the laser diode and receiver lens stayed aligned during testing.

### Tuning Signal Logic and LED Brightness

I first verified the alarm behavior using standard digital outputs. Once the signal logic was working, I moved the green status indicator to a PWM-capable Arduino pin and used `analogWrite()` to dim it. This let me balance the brightness of the secure and alarm states in software without changing the physical resistors.

## Key Takeaways

- **Sensor calibration:** Learned to tune an optical module's onboard potentiometer and understand how its sensitivity threshold determines a digital output.
- **Signal control:** Applied digital input logic and PWM output to create distinct, readable system states.
- **Practical prototyping:** Saw how wiring strain and physical alignment can affect sensor reliability just as much as the circuit logic.
- **Working with unfamiliar modules:** Used iterative testing to understand a kit component's behavior before integrating it into a larger circuit.
