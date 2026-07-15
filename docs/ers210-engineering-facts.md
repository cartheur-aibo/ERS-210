# ERS-210 Engineering Facts Baseline

## Purpose

This note condenses the most immediately useful engineering facts from
`ModelInformation_210_E.pdf` into a quick reference for future reverse
engineering, sidecar design, and live-experiment planning.

Primary source:

- `docs/ModelInformation_210_E.pdf`

This is not a service manual. It is a Sony `OPEN-R SDK` model-information
document.

## Mechanical Summary

### Body measurements called out in the Sony model document

- overall length figure shown: `289 mm`
- overall height figure shown: `152 mm`
- head pan center, tilt center, and roll center are explicitly dimensioned in
  the document
- additional sub-dimensions are provided for body and leg geometry

For exact geometry, refer to the dimension drawings in:

- [ModelInformation_210_E.pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ModelInformation_210_E.pdf)

### Degrees of freedom

- head: `3 DOF`
  - pan
  - tilt
  - roll
- ears: `1 DOF x 2`
- chin / mouth: `1 DOF`
- front legs: `3 DOF x 2`
- rear legs: `3 DOF x 2`
- tail: `2 DOF`

Total called out in the document:

- head section total: `6 DOF`
- legs total: `12 DOF`
- tail total: `2 DOF`

## Device Layout

### Output devices

The document identifies these output devices:

- mode display LED, green
- eye LEDs, red and green
- speaker
- tail LED, blue and orange
- chest light, green and orange
- inside body clock display LCD
- `Memory Stick` access lamp
- piezoelectric buzzer for boot and shutdown sound

### Input devices

The document identifies these input devices:

- head touch sensor
- back touch sensor
- chin touch sensor
- paw touch sensor, one per leg
- range sensor
- color camera
- stereo microphones, left and right
- pause button
- acceleration sensor
- vibration sensor
- thermo sensor
- clock and setting switch
- PC Card slot, `PCMCIA Type`
- `Memory Stick` slot

## Block-Level Structure

The document’s block overview explicitly shows these major physical blocks:

- head block
- left front leg block
- right front leg block
- left rear leg block
- right rear leg block
- tail block
- `OPEN-R bus connector`

That matters because it suggests a modular internal structure rather than one
single monolithic body harness.

## Head Subassembly Clues

The head configuration figure explicitly shows:

- touch sensor
- LED board
- color camera
- pan motor
- tilt motor
- roll motor
- mouth motor
- ears
- pause button

This is useful because it indicates the head is already a strongly integrated
subassembly with multiple actuators and sensors in one mechanical unit.

## Leg Subassembly Clues

Each leg block is shown with:

- `J1` motor and potentiometer
- `J2` motor and potentiometer
- `J3` motor and potentiometer
- paw touch sensor
- `OPEN-R bus connector`

This is one of the strongest official clues we currently have for low-level
architecture:

- the joints appear to use potentiometer feedback
- each leg is a repeatable 3-joint module
- each leg participates in the `OPEN-R` bus structure

## CPC Primitive Locator Map

The ERS-210 document provides official `CPC Primitive Locator` names for many
body parts and devices. These are especially useful when aligning software
assumptions with physical robot structure.

### Head and face related

- neck tilt
- neck pan
- neck roll
- mouth
- head sensor back
- head sensor front
- chin switch
- PSD position sensing device
- microphone
- speaker
- color camera
- left ear
- right ear
- eye lights
- mode indicator

### Legs

For each leg, the document lists:

- `J1`
- `J2`
- `J3`
- paw sensor

### Tail and tail lights

- tail pan
- tail tilt
- tail light blue
- tail light orange

The existence of these official locator names is important because it confirms
that the software layer already sees the robot as a named hierarchy of joints,
sensors, and emitters.

## Reverse-Engineering Implications

### What this document helps with

- identifying the major modules of the robot
- understanding which actuators and sensors exist
- understanding rough kinematic limits
- understanding that leg joints use potentiometer feedback
- understanding the software-visible naming hierarchy
- confirming that the body is partitioned around `OPEN-R`-visible structures

### What this document does not provide

- board schematic
- motherboard connector pinout
- bus voltage levels
- bus protocol framing
- motor driver part numbers
- CPU pinout
- power tree
- repair or disassembly instructions

## Why This Matters For Volatco

For sidecar work, this document is enough to support:

- naming the joints and sensors we want to reason about
- choosing low-risk first behaviors
- planning posture-safe experiments
- understanding where the body is modular

For full `GA144-runs-the-robot` ambitions, it is not enough. It gives us a map
of the body at the `OPEN-R` model level, not the electrical replacement level.

## Immediate Working Conclusions

- The ERS-210 is internally modular at least at the block level.
- Legs are 3-joint repeated modules with potentiometer feedback.
- The head is a dense integrated subassembly with multiple motors and sensors.
- `OPEN-R bus connector` language is real and explicit in the Sony document.
- The software-visible primitive hierarchy is official and useful.
- None of this yet proves that a Volatco board can replace the native control
  electronics directly.

## Recommended Next Use

Use this note together with:

- [ga144-runs-the-robot-checklist.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ga144-runs-the-robot-checklist.md)
- [how-to-sidecar-volatco-with-ers210.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/how-to-sidecar-volatco-with-ers210.md)

That combination gives us:

- the official model-level facts
- the practical sidecar path
- the checklist for any deeper CPU-replacement ambition
