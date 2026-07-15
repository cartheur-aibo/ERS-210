## ERS-210

This repo is becoming a working ERS-210 research and preservation notebook,
with two near-term goals:

- build a home-brewed ERS-210 service manual from teardown evidence
- determine the safest realistic path for Volatco / `GA144` sidecar or deeper
  grafting experiments

## What Has Been Done

We now have a stronger technical baseline than we started with.

### Official and reference documents gathered

- Sony ERS-210 and ERS-220 `Model Information` PDFs copied locally:
  - [docs/ModelInformation_210_E.pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ModelInformation_210_E.pdf)
  - [docs/ModelInformation_220_E.pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ModelInformation_220_E.pdf)
- existing local user and accessory manuals retained in `docs/`

### ERS-210 engineering baseline extracted

- [docs/ers210-engineering-facts.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ers210-engineering-facts.md)

This gives us:

- body measurements
- device layout
- joint and sensor structure
- `OPEN-R` block and primitive naming
- an official model-level baseline for later reverse engineering

### Volatco / GA144 planning notes written

- [docs/volatco-ers210-sidecar-proposal.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/volatco-ers210-sidecar-proposal.md)
- [docs/volatco-ers210-bridge-interface-sketch.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/volatco-ers210-bridge-interface-sketch.md)
- [docs/how-to-sidecar-volatco-with-ers210.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/how-to-sidecar-volatco-with-ers210.md)
- [docs/ga144-runs-the-robot-checklist.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ga144-runs-the-robot-checklist.md)

These documents establish:

- sidecar-first as the credible immediate path
- direct CPU replacement as a reverse-engineering project, not a wiring task
- the checklist for any future claim that `GA144` can directly run the robot

### Teardown material added

- [teardown/ERS-210 Body Shell Disassembly .pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Body%20Shell%20Disassembly%20.pdf)
- [teardown/ERS-210 Head Shell Disassembly .pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Head%20Shell%20Disassembly%20.pdf)
- [teardown/ERS-210 Tail Module Disassembly.pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Tail%20Module%20Disassembly.pdf)

### House-manual strategy started

- [docs/disassembly-and-reporting-strategy.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/disassembly-and-reporting-strategy.md)
- [docs/disassembly-body-shell.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/disassembly-body-shell.md)

This gives us the first structure for building our own service manual from
direct evidence rather than relying only on Sony or community documents.

## What To Do Next

The next phase is practical documentation work on the real robot.

### 1. Disassemble carefully, one subsystem boundary at a time

Recommended first order:

- body shell
- head shell
- tail module

Do not jump straight to a full uncontrolled teardown.

### 2. Photograph everything

For each subsystem, capture:

- assembled state
- every exposed screw pattern
- hidden fasteners
- clips and release directions
- cable routing before unplugging
- every separated part
- any cracks, warping, degraded rubber, or adhesive

### 3. Parse each session into markdown

After each session, convert observations into repo-native notes:

- step order
- hidden catches
- screw counts and sizes
- fragile points
- safe stopping points
- reassembly notes
- `Observed / Inferred / Needs verification`

### 4. Build a home-brewed service manual

The working target is a comprehensive internal manual that covers:

- shell disassembly
- head disassembly
- tail module disassembly
- fastener map
- connector and cable notes
- reassembly checkpoints
- observed revision differences

### 5. Use that manual to guide Volatco grafting work

Only after disassembly and documentation mature should we push further into:

- internal connector identification
- power tree mapping
- board and cable labeling
- bus observation
- sidecar access-point selection
- any deeper `GA144` grafting ambitions

## Working Principle

The service-manual work and the Volatco-grafting work are linked:

- disassembly gives us physical evidence
- photographs preserve evidence
- parsed markdown turns evidence into reusable procedure
- that procedure becomes the house manual
- the house manual becomes the foundation for safe controller experiments

## Current Direction

At this stage, the right mindset is:

- preserve first
- document second
- infer carefully
- experiment only after the robot’s structure is better understood

![explode](/images/exploded.jpg)
