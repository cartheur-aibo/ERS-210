# Checklist For A True GA144-Controlled ERS-210

## Purpose

This checklist defines what must be known before a Volatco `GA144` can credibly
be expected to run a Sony AIBO ERS-210 as more than a sidecar supervisor.

This is not a sidecar checklist.
This is the harder checklist for the stronger claim:

- `GA144` is directly responsible for running the robot body

At the current state of evidence, that goal is still a reverse-engineering
project, not a wiring task.

## Current Evidence Baseline

We do have:

- consumer and accessory manuals for ERS-210
- official Sony `OPEN-R SDK` model information documents for ERS-210 and ERS-220
- software-layer OPEN-R documentation
- community historical knowledge about common failure modes such as DHS

We do not yet have:

- a verified Sony ERS-210 service manual
- a motherboard schematic
- a CPU pinout
- an internal bus description
- a documented motor-controller protocol
- a board-level power tree

That means the correct posture right now is:

- do not assume a direct electrical graft path already exists

## The Minimum Knowledge Required

### 1. Power domain map

We need a verified answer to all of these:

- what voltage enters from the battery pack path
- how that power is split between logic and motor domains
- which regulators exist on-board
- where inrush, startup, and undervoltage behaviors are enforced
- whether battery authentication or battery-state signaling exists
- what current peaks occur during startup and motion

Exit condition:

- one written power tree with measured or strongly evidenced voltages and rails

### 2. Boot path and reset sequencing

We need to know:

- what component boots first
- how reset is distributed
- whether multiple control processors participate in startup
- whether actuator controllers come up independently
- whether `Memory Stick` contents influence early hardware initialization
- what conditions force pause, clinic, or no-motion states

Exit condition:

- one boot sequence timeline from power application to motion-capable state

### 3. CPU and companion processors

We need to identify:

- the main CPU part
- any microcontrollers or servo controllers
- battery-management microcontrollers
- sensor-side support ICs
- any buses between those devices

Exit condition:

- one component map of the main board and any satellite boards

### 4. Internal communication buses

We need to learn:

- which buses connect CPU to motor control
- which buses connect CPU to sensors
- whether buses are serial, parallel, custom, or mixed
- voltage levels and timing
- idle states, framing, and error behavior

Exit condition:

- one protocol table per internal bus with at least partial captures or trace evidence

### 5. Actuator command semantics

We need to know:

- whether the main CPU sends position targets, PWM, torque-like values, or mode words
- update rates for each actuator path
- how joint limits are enforced
- how pause and recovery override normal commands

Exit condition:

- one description of what must be emitted for the robot to move safely

### 6. Sensor data semantics

We need to know:

- how touch, distance, camera, microphone, acceleration, vibration, and pause inputs are represented internally
- whether raw signals are preprocessed before reaching the main control layer
- which sensors are safety-critical

Exit condition:

- one map from physical sensor to observed internal representation

### 7. Safety and fault behavior

We need to understand:

- what causes the robot to enter pause
- what faults inhibit motion
- how overcurrent, thermal, or stall conditions are handled
- whether a missing or bad control message causes a safe stop
- what happens if one subsystem boots late or fails

Exit condition:

- one fault matrix covering at least the major safety transitions

### 8. Mechanical constraints on replacement

We need to know:

- whether a Volatco-based replacement could physically fit
- how cables and harnesses route in the ERS-210 body
- where an added board could be mounted without blocking service access
- whether connector count and geometry make a drop-in board unrealistic

Exit condition:

- one mechanical feasibility note with clear no-go areas

## Practical Work Plan

### Phase A: Documentation harvest

Tasks:

- extract all relevant facts from `ModelInformation_210_E.pdf`
- compare against `ModelInformation_220_E.pdf`
- extract all ERS-210 operating constraints from local manuals
- collect every mention of clinic mode, pause, charging, and service behavior

Deliverables:

- ERS-210 engineering facts note
- ERS-210 vs ERS-220 differences note

### Phase B: Visual hardware survey

Tasks:

- photograph exterior service access points
- photograph board assemblies after careful disassembly
- identify chips, connectors, daughtercards, and cable routes
- label every visible connector and board marking

Deliverables:

- board photo atlas
- component and connector inventory

### Phase C: Passive electrical characterization

Tasks:

- continuity mapping of power and ground
- regulator identification
- power rail measurement during safe bring-up
- identify likely logic-voltage domains

Deliverables:

- preliminary power tree
- rail table

### Phase D: Bus observation

Tasks:

- observe startup signals non-invasively if possible
- identify recurring digital traffic between CPU and other controllers
- correlate traffic with pause, wake, and simple behavior transitions

Deliverables:

- trace captures
- candidate bus map

### Phase E: Command-path interpretation

Tasks:

- induce known safe behaviors
- correlate behaviors with bus activity
- infer command granularity and update rates

Deliverables:

- candidate actuator command model

### Phase F: Replacement feasibility decision

Decision question:

Can a Volatco `GA144` realistically replace the main control role without an
intermediate native ERS-210 controller remaining in charge?

Possible outcomes:

- `No`: remain with sidecar architecture
- `Partial`: let GA144 supervise one subsystem only
- `Yes, with major custom interface board`: proceed only with explicit risk acceptance

## Stop Conditions

The project should stop and fall back to sidecar mode if any of these hold:

- the internal buses are too complex or safety-critical to reproduce
- the timing requirements exceed what can be bridged safely
- the power sequencing is tightly coupled to undocumented Sony hardware
- the mechanical integration would require destructive irreversible changes
- the only robot would face disproportionate damage risk

## What The Deeper Search Found

The broader search did not produce a verified Sony factory service manual or
board schematic for ERS-210.

The strongest official technical documents currently available in this workspace
remain:

- `docs/ModelInformation_210_E.pdf`
- `docs/ModelInformation_220_E.pdf`

These documents provide:

- external appearance drawings and measurements
- operational limits
- device layout
- configuration overview
- CPC primitive locators
- servo gain references
- sensor and output device descriptions

That is valuable engineering material, but it is still not enough to justify a
direct CPU graft.

## Final Decision Gate

Do not claim that `GA144` can run the ERS-210 directly until all of the
following are true:

- power tree understood
- boot path understood
- internal controllers identified
- bus traffic observed
- actuator command semantics partially decoded
- safety fallback behavior understood
- mechanical feasibility demonstrated

Before that point, the credible architecture is still:

- `GA144` sidecar
- native ERS-210 body control retained
