# Volatco to ERS-210 Bridge Interface Sketch

## Purpose

This note sketches the minimum practical interface between a Volatco sidecar
and a live Sony AIBO ERS-210. It is intended to follow the sidecar proposal and
to define the first bridge boundary without assuming a direct replacement of the
ERS-210 main CPU.

The guiding principle is simple:

- Volatco decides behavior state transitions.
- The ERS-210 native environment executes body-specific motion and platform
  services.

## What The AIBOFTP Archive Adds

The archive at `/home/cartheur/Downloads/AIBOFTP/` does not appear to contain a
service manual, motherboard schematic, or CPU pinout for the ERS-210.
However, it does provide several useful constraints:

- `210AW01WW_LIFE_UG.pdf` confirms the native `Memory Stick` boot and operating
  path, the use of the pause button, and the fact that connecting the AC
  adaptor to AIBO's AC adaptor plug suppresses normal autonomous behavior.
- `210P1WW_STTN_UG.pdf` confirms that the Energy Station charges through
  charging terminals on AIBO's stomach and uses the `ERA-201B1` battery pack
  and `ERA-201P1` AC adapter family.
- `AIBO SPEED BOARD ERA-210TP2.pdf` confirms a mechanically meaningful stomach
  mating area and repeated use of the underside charging-terminal region as a
  handled surface that must not be contaminated or obstructed.
- `AIBO SOFTWARE AND ACCESSORIES GUIDE ERS-210.pdf` confirms the practical
  accessory ecosystem around the ERS-210, including the programming
  `Memory Stick`, wireless LAN card, and Energy Station.

Taken together, these documents push us toward a bridge that is software-first,
power-safe, and mechanically non-destructive.

## Recommended Bridge Layers

### Layer 1: Primary bridge

The first bridge should be a logical control bridge running through the native
OPEN-R and `Memory Stick` execution path.

Volatco role:

- evaluate behavior-diagram state transitions
- run small reactive policies
- emit a compact command vocabulary
- supervise heartbeat and timeout behavior

ERS-210 role:

- boot native code from `Memory Stick`
- receive bridge commands
- map commands to safe robot-specific actions
- retain authority over posture, pause, and stop behavior

This is the recommended first bridge because it does not require opening the CPU
path or impersonating Sony board-level hardware.

### Layer 2: Optional physical sideband

If a direct signal path is later needed, use a tiny sideband only for:

- heartbeat
- emergency stop
- one or two trigger lines

This sideband should not attempt to carry raw joint-control traffic in early
phases.

### Layer 3: Power boundary

The robot and Volatco should not share a casual power architecture.

Recommended arrangement:

- ERS-210 powered via battery emulator or equivalent replacement pack
- Volatco powered from its own regulated source
- shared ground only when a bridge signal requires it
- current measurement included during bring-up
- hard motion-power cutoff available to the operator

## Minimum Command Vocabulary

The first bridge should expose a deliberately tiny contract.

Suggested commands from Volatco to ERS-210:

- `PING`
- `STOP`
- `SAFE_POSTURE`
- `RUN_BEHAVIOR <id>`
- `SET_MODE <id>`
- `REQUEST_STATUS`

Suggested status from ERS-210 to Volatco:

- `READY`
- `BUSY`
- `SAFE`
- `PAUSED`
- `LOW_POWER`
- `FAULT`
- `BEHAVIOR_DONE <id>`

This gives us enough structure to test real embodied state transitions without
creating a large protocol too early.

## First Physical Integration Candidates

These are ordered from safest to riskiest.

### Candidate A: Memory Stick plus native software bridge

Description:
Use a native ERS-210 program loaded through the programming `Memory Stick` as a
bridge object. Volatco communicates with that bridge through whatever low-risk
channel proves practical in later inspection.

Why it is attractive:

- aligned with documented AIBO operating model
- keeps motion execution inside the native robot stack
- easiest place to preserve safe pause and recovery behavior

Current limitation:

- we still need to define the actual transport path between Volatco and the
  bridge object

### Candidate B: Wireless bridge

Description:
Use the AIBO wireless LAN ecosystem and a native ERS-210 bridge object for
command transfer.

Why it is attractive:

- no invasive physical wiring into the robot body
- fits the existing OPEN-R and accessory ecosystem

Current limitation:

- depends on working wireless LAN hardware and software path
- adds more runtime complexity than a minimal local bridge

### Candidate C: Minimal wired sideband

Description:
Use a small hardware sideband for only a few external signals, while native
software still owns motion semantics.

Why it is attractive:

- deterministic trigger timing
- simpler than trying to stream richer data at the board level

Current limitation:

- no verified service connector or logic-level map yet
- requires careful voltage and isolation planning

### Candidate D: Direct internal bus or motherboard attachment

Description:
Attach Volatco deeper into the ERS-210 internals.

Recommendation:
Do not start here.

Reason:

- no verified CPU pinout in the local archive
- no confirmed motor-bus timing or reset sequencing
- highest chance of damaging the robot

## Mechanical Guidance From The Archive

Even without a service manual, the archive gives some useful mechanical clues.

### 1. Stomach region is an active interface zone

The Energy Station guide states that charging occurs through terminals on
AIBO's stomach. The Speed Board guide also uses the stomach area as the mating
surface for an external accessory.

Implication:

- avoid blocking or contaminating the stomach charging-terminal area
- do not assume that the stomach zone is a safe place for arbitrary new wiring
- if an external mount is needed, it should respect known accessory geometry

### 2. Memory Stick and battery access are established service actions

The operating guides repeatedly use the underside cover, `Memory Stick` slot,
and battery insertion path as supported operator access points.

Implication:

- these access points are safer targets for experimental workflows than opening
  deeper internal board assemblies
- the first bridge should be designed around those existing service paths

### 3. Early experiments should avoid unstable full-body locomotion

The Speed Board documentation repeatedly emphasizes supervision, hard surfaces,
and avoidance of unsafe forcing during motion.

Implication:

- the first live Volatco experiments should focus on constrained posture,
  head/face actions, sound, or low-energy transitions instead of walking

## Proposed First Bridge Contract

This is the smallest bridge I would actually implement first.

Volatco responsibilities:

- keep a local behavior state machine
- send one command at a time
- require an acknowledgement before sending the next state-driving command
- emit `STOP` immediately on timeout or inconsistent status

ERS-210 bridge responsibilities:

- accept only known commands
- reject commands when unsafe
- translate abstract commands into native action routines
- report a coarse state back to Volatco
- force pause or safe posture on bridge loss

Operator responsibilities:

- keep the robot physically supported during earliest tests
- monitor current draw and temperature during power bring-up
- retain immediate access to a hard stop

## Recommended First Experiment

The first real bridge test should be:

1. Power ERS-210 from the battery-emulator path.
2. Boot a native bridge-capable `Memory Stick`.
3. Start Volatco heartbeat only.
4. Send `REQUEST_STATUS`.
5. Send one harmless command such as `SAFE_POSTURE` or one head-motion trigger.
6. Confirm `BEHAVIOR_DONE`.
7. Force a heartbeat timeout and verify safe fallback.

Success on this sequence would validate the integration concept without
requiring direct motherboard intervention.

## What Still Needs To Be Discovered

Before choosing between a wireless bridge and a tiny wired sideband, we still
need:

- a confirmed battery-emulator wiring plan
- a confirmed bridge transport path
- a confirmed logic-level compatibility plan for any wired signals
- a confirmed list of native ERS-210 behaviors that can be safely triggered

## Recommendation

Build the first Volatco-to-ERS-210 bridge as a native-software-controlled
sidecar integration, not as a direct electrical CPU graft.

The archive supports that decision. It gives us real evidence for the supported
service boundaries of the ERS-210, but it does not provide the kind of board
schematic or internal bus documentation that would justify a CPU-level
replacement attempt.
