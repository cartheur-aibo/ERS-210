# Volatco Sidecar Proposal for Sony AIBO ERS-210

## Purpose

This proposal outlines a practical path for using a Volatco board as an
external experimental coprocessor with a live Sony AIBO ERS-210. The goal is
to extend behavior-diagram and SOFTSIM-style work into real robotic tests
without attempting an unsafe or premature replacement of the ERS-210 main CPU.

The key design choice is to treat Volatco as a sidecar controller rather than a
drop-in motherboard substitute. This preserves the original OPEN-R control
stack while giving us a low-latency external platform for behavior evaluation,
watchdog logic, sensor experiments, and staged hardware-in-the-loop research.

## Why Not Replace the Main CPU

At present, a direct CPU replacement is not recommended.

Reasons:

- The ERS-210 boot chain, power sequencing, and internal board-to-board
  signaling are not yet fully reverse engineered in this workspace.
- OPEN-R expects native system objects such as power management, virtual robot,
  and designed robot services to exist within the original platform runtime.
- The Volatco board is a general exposed-header asynchronous compute platform,
  not a known electrical or protocol-level substitute for the ERS-210 CPU
  module.
- A failed direct graft could damage actuator control, battery handling, or
  safety behavior in a difficult-to-recover robot.

The live-test objective is still achievable, but the safe first step is a
sidecar integration.

## Core Hypothesis

Behavior diagrams derived from ERS-111 and related OPEN-R work can serve as an
intermediate control representation that runs partly outside the stock robot.
Volatco can host compact reactive control logic and state transitions while the
ERS-210 continues handling its native low-level actuation and platform-specific
services.

This allows us to test whether behavior-state abstractions from the simulator
and diagram corpus can drive meaningful embodied experiments on a real robot.

## Proposed System Architecture

### 1. Native robot remains intact

The ERS-210 continues to provide:

- low-level actuator control
- joint safety and posture management
- sensor acquisition through the native stack
- boot and device initialization
- OPEN-R object loading from Memory Stick

### 2. Volatco acts as an external behavior coprocessor

Volatco provides:

- behavior-state execution derived from SOFTSIM or diagram logic
- event filtering and arbitration
- timing-sensitive watchdog supervision
- experiment logging and repeatable test harness logic
- optional external sensor fusion for future expansions

### 3. A narrow bridge links the two systems

The bridge should initially be minimal and conservative. Good first candidates
are:

- a serial command path
- a simple GPIO event path
- a small command vocabulary such as posture select, motion trigger, stop, and
  status request

The bridge should avoid direct raw motor-driving in the first phase.

## Power Strategy

The ERS-210 in this project currently has no battery pack installed. The local
documentation indicates that charging through the AC adaptor plug does not leave
the robot in a normal autonomous operating mode. For live experiments, the
robot should therefore be powered through a battery-emulator approach rather
than solely through the charging terminal.

Recommended power arrangement:

- ERS-210 powered by a battery-bay emulator or equivalent replacement pack
- Volatco powered independently from its own regulated supply
- grounds tied together only when needed for signaling
- inline current measurement during early bring-up
- a hardware kill path that removes robot motion power quickly

This decouples robot survival from Volatco stability and makes failures easier
to contain.

## Safety Requirements

The first live experiments must be designed around graceful failure.

Mandatory safeguards:

- one physical emergency stop or power cutoff
- one software stop command recognized by the robot-side bridge
- startup posture that minimizes fall risk
- no unsupported free-walking in earliest tests
- watchdog timeout that returns the robot to pause, rest, or no-motion state
- bench or cradle support during earliest powered trials

If there is any ambiguity about actuator ownership, the native robot must win.

## Phase Plan

### Phase 0: Documentation and electrical reconnaissance

Objective:
Define a safe signal and power map before any live wiring.

Tasks:

- document the ERS-210 battery-bay power approach
- identify a non-destructive access point for bridge signals
- confirm shared-ground and logic-level assumptions
- inspect internal connectors and photograph board topology
- determine whether a serial or service connector is exposed enough to use

Exit criteria:

- one agreed wiring sketch
- one agreed supply plan
- one written stop/recovery procedure

### Phase 1: Robot-only baseline

Objective:
Verify that the ERS-210 can be powered safely and repeatably without Volatco in
the loop.

Tasks:

- bench-power the ERS-210 through the battery emulator
- verify pause, startup, and shutdown behavior
- confirm Memory Stick boot path
- identify one repeatable behavior or motion sample that can be triggered

Exit criteria:

- repeatable power-up without abnormal heat or resets
- repeatable safe stop procedure
- one reproducible stock behavior test

### Phase 2: Bridge stub

Objective:
Create the thinnest possible control bridge between Volatco and the ERS-210.

Tasks:

- implement a minimal Volatco-side state machine
- define a tiny robot command vocabulary
- connect only the required bridge lines
- test idle signaling and heartbeat without movement

Exit criteria:

- stable heartbeat for an extended run
- watchdog behavior demonstrated
- no unintended motion

### Phase 3: Assisted live behaviors

Objective:
Use Volatco to supervise or trigger stock robot motions in a controlled setting.

Tasks:

- map one or two behavior-diagram states onto real triggers
- run seated or constrained posture experiments
- log event timing, transitions, and recovery behavior
- compare live transitions to simulator expectations

Exit criteria:

- one complete behavior loop exercised live
- bounded recovery from bridge interruption
- timing notes recorded for each transition

### Phase 4: Expanded embodied experiments

Objective:
Move from trigger-level integration toward richer behavior composition.

Possible expansions:

- external sensor input handled by Volatco
- arbitration between multiple behavior fragments
- adaptive switching between state clusters
- real-time supervisor for embodied state experiments

This phase should only begin after stable earlier phases.

## First Experiment Recommendation

The safest first real experiment is not walking. It is a constrained,
low-energy behavior sequence such as:

- wake or posture transition
- head movement or ear movement
- LED or sound cue
- stop and return to safe state

This is enough to validate:

- the battery-emulator power path
- the bridge signal path
- the timing model
- the behavior-diagram-to-robot mapping concept

## Integration Boundary

To keep the project tractable, the first proposal assumes the following
boundary:

- Volatco decides what state transition or experiment step should happen next.
- The ERS-210 native environment decides how to execute body-specific motion
  safely.

That boundary is the main reason this proposal is realistic. It avoids forcing
Volatco to impersonate Sony hardware before we have sufficient reverse
engineering evidence.

## Main Technical Unknowns

These unknowns should be treated as explicit research items:

- exact ERS-210 internal connector and bus roles
- acceptable external logic levels on candidate signal points
- whether a practical serial or service path can be reused
- how much of the desired behavior vocabulary can be expressed through native
  command triggers
- whether motion latency is low enough for the intended behavior experiments

If any of these fail, the proposal can still succeed at a narrower scope such as
behavior observation, logging, or supervisory control.

## Success Criteria

This project should be considered a success if it demonstrates all of the
following:

- the ERS-210 can be powered safely without its original battery pack
- Volatco can participate in live experiments without replacing the main CPU
- at least one behavior-diagram fragment can be mapped to a repeatable embodied
  test
- failures fall back to a safe robot state
- the setup produces actionable information for future deeper integration

## Recommendation

Proceed with a Volatco sidecar architecture, not a main-CPU replacement.

The recommended next engineering step is to create a battery-emulator plan and a
bridge-interface sketch for the ERS-210, then run a Phase 1 robot-only bring-up
before any live Volatco wiring.
