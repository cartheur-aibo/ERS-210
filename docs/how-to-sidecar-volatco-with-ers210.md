# How To Sidecar Volatco With ERS-210

## Purpose

This note answers a narrower question than the proposal documents:

How would we actually sidecar a Volatco board to a Sony AIBO ERS-210 without
pretending to replace the main CPU?

The short answer is:

- do not wire Volatco in as the robot's CPU
- do wire Volatco in as an external coprocessor or supervisor
- keep the ERS-210 in charge of body-specific motion and safety

## The Plain-English Model

Think of the first sidecar setup like this:

- the ERS-210 remains "the body"
- Volatco becomes "the experiment controller"
- a small bridge passes simple commands and status between them

Volatco should decide things like:

- what behavior state comes next
- whether a timeout has happened
- whether a simple external condition should trigger a change

The ERS-210 should still decide things like:

- how to move its joints safely
- how to pause
- how to recover to a safe posture
- how to interpret native sensor and actuator semantics

## The Three Sidecar Patterns

### Pattern 1: Software sidecar over native ERS-210 runtime

This is the cleanest first version.

Architecture:

```text
Volatco
  |
  | simple command/status link
  v
Native ERS-210 bridge object on Memory Stick
  |
  | OPEN-R / native object calls
  v
ERS-210 motion, sensors, posture, pause, safety
```

What this means:

- Volatco runs behavior logic externally.
- The ERS-210 boots a native bridge program from `Memory Stick`.
- That bridge program receives commands and triggers safe native routines.

Good for:

- first live experiments
- low-risk behavior tests
- proving the behavior-diagram mapping idea

Not good for:

- direct actuator waveform experiments
- replacing Sony control loops

### Pattern 2: Software sidecar plus tiny wired sideband

This is the next step if timing or observability becomes important.

Architecture:

```text
Volatco
  |\
  | \__ heartbeat / stop / trigger lines
  |
  | simple command/status link
  v
Native ERS-210 bridge object on Memory Stick
  |
  v
ERS-210 native motion and safety
```

What this adds:

- one heartbeat line
- one emergency stop or inhibit line
- one or two trigger lines

Good for:

- watchdog experiments
- bounded-latency triggers
- proving recovery behavior

Not good for:

- deep motherboard grafting
- raw motor-bus substitution

### Pattern 3: Mechanically mounted external sidecar

This is not a new control model. It is just a more mature packaging of Pattern
1 or Pattern 2.

Architecture:

```text
[ Volatco sidecar module ]
        |
        | mounted externally on harness, cradle, or test fixture
        |
[ ERS-210 body ]
```

Good mounting targets:

- a bench fixture
- a support frame
- a removable saddle or harness that avoids the stomach charging terminals

Bad mounting targets:

- directly over the stomach charging contact area
- any area that blocks battery access or `Memory Stick` service access
- any mount that loads the neck, head, or legs mechanically

## What Gets Wired

In the first realistic sidecar, the wiring should be minimal.

### Power wiring

Use two power domains:

- ERS-210 power domain
- Volatco power domain

Recommended arrangement:

```text
Bench supply or battery-emulator path ---> ERS-210 battery input path
Separate regulated supply             ---> Volatco J1 power input
Shared ground only if a signal link requires it
```

Important:

- do not depend on the ERS-210 AC adaptor plug for normal live operation
- do not power Volatco from arbitrary robot internals
- do not power the ERS-210 from Volatco

### Signal wiring

For the first hardware sidecar, keep signals to this scale:

- `GND`
- `HEARTBEAT`
- `STOP` or `INHIBIT`
- `TRIGGER_A`
- optional `STATUS_A`

That is the right order of magnitude. Not a full bus replacement.

### Software bridge

The bridge running on ERS-210 should translate a very small external contract:

- `PING`
- `STOP`
- `SAFE_POSTURE`
- `RUN_BEHAVIOR <id>`
- `REQUEST_STATUS`

Return values:

- `READY`
- `BUSY`
- `SAFE`
- `PAUSED`
- `FAULT`
- `DONE`

## What Does Not Get Wired First

This part matters just as much as what does get wired.

Do not start by wiring Volatco directly to:

- ERS-210 main CPU pins
- raw joint motor drive lines
- internal power rails you have not characterized
- unknown internal buses
- the stomach charging terminals as if they were a general data interface

Those are later reverse-engineering targets, not first sidecar targets.

## First Real Sidecar Build

If I were building the first experiment, I would do exactly this:

### Step 1: keep the robot stock internally

- stock ERS-210 body
- stock actuator chain
- stock native runtime expectations

### Step 2: power the robot through a battery-emulator path

- no battery required
- no dependence on charge-port-only operation

### Step 3: mount Volatco off-body or on a removable fixture

- bench fixture first
- robot harness second
- direct body mount later if needed

### Step 4: run a tiny bridge contract

- Volatco emits one command at a time
- ERS-210 acknowledges
- timeout forces stop or safe posture

### Step 5: choose a low-risk behavior

Start with one of these:

- head move
- ear move
- light cue
- sound cue
- safe posture transition

Do not start with:

- walking
- self-righting
- fast repeated motion
- anything that can yank the robot off a bench

## The Simplest Viable Sidecar Diagram

This is the most concrete first-version sketch:

```text
                        +----------------------+
                        |      Volatco         |
                        | behavior state logic |
                        | watchdog / timeout   |
                        | simple supervisor    |
                        +----------+-----------+
                                   |
                      command/status|link
                                   |
                      +------------v-------------+
                      | ERS-210 bridge object    |
                      | on programming Memory    |
                      | Stick / native runtime   |
                      +------------+-------------+
                                   |
                             native calls
                                   |
                +------------------v------------------+
                | ERS-210 native body functions       |
                | posture, joints, pause, sensors,    |
                | native safety and motion execution  |
                +-------------------------------------+


Bench supply / battery emulator ----> ERS-210 power path
Separate supply --------------------> Volatco power path
```

## If We Must Add One Wire-Level Sideband

If you want a literal hardware sidecar before we know the internal buses, the
most conservative version is this:

```text
Volatco:
  OUT1 -> HEARTBEAT
  OUT2 -> STOP
  OUT3 -> TRIGGER
  GND  -> common signal ground

ERS-210 bridge side:
  reads or interprets those as a tiny external supervisor contract
```

This still assumes the ERS-210 side has a safe, known way to see those signals.
That access path is still one of the open engineering questions.

## Practical Recommendation

If the question is "how do we sidecar Volatco," the first answer should be:

- sidecar it logically first
- sidecar it mechanically second
- sidecar it electrically only in a tiny supervised way
- do not sidecar it as a motherboard replacement

That is the version most likely to get us a real experiment without damaging the
only ERS-210 platform currently available.
