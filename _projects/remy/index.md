---
layout: post
title: Remy the Rat — Animatronic Headband
description: Built a wearable animatronic Remy the Rat using a Teensy 3.6, two micro servos, and wire linkages to animate the character's arms.
skills:
  - Teensy 3.6
  - C++
  - Embedded Systems
  - Servo Control
  - 3D Printing
  - Mechanical Integration
---

## Project Demonstration

The first video shows the project in action.

{% include youtube-video.html id="1TSq6OtyHCs" autoplay="false" %}

## Overview

Built an animatronic Remy the Rat headband featuring two servo-driven arms that move and interact with the wearer's hair. The project combines a 3D-printed character, wire-based mechanical linkages, and embedded control.

## Mechanical Design

- Used the STL model from [She Builds Robots' Remy Model](https://github.com/shebuildsrobots/remy_model).
- Adapted the original servo-and-wire linkage concept, connecting the servo-driven mechanisms to Remy's hands.
- Used two micro servos, with one servo controlling each arm.

## Embedded Systems & Control

- Used a Teensy 3.6 microcontroller to control the servos through digital pins 2 and 3.
- Programmed the repeating "67" idle animation in C++ using the Arduino Servo library.
- Alternated the arm movements by commanding the two servos to move between their calibrated up and down positions.
- Accounted for the mirrored physical mounting of the servos by defining separate position values for the left and right servos.

## Additional Demonstration

{% include youtube-video.html id="xMDeFViIPk0" autoplay="false" %}

## Hardware Documentation

*Additional build documentation coming soon.*

- Wiring diagram
- Photo of the rear assembly showing the Teensy, servos, and wire linkages

## Planned Development

Plan to integrate a GY-521 module (based on the MPU-6050) to measure head orientation and use head tilt to control Remy's arm movements. This will add an interactive mode alongside the existing idle animation.

## Acknowledgments

Inspired by [She Builds Robots' Remy Model](https://github.com/shebuildsrobots/remy_model). This project uses the original STL model and adapts its servo-and-wire linkage concept. The original design and model are credited to its creator.
