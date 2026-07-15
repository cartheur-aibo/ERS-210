# ERS-210 Disassembly And Reporting Strategy

## Purpose

This note defines how we should turn the ERS-210 teardown material in this repo
into a durable, comprehensive in-house manual.

The immediate goal is not merely to take the robot apart.
The goal is to produce a repeatable body of evidence that supports:

- safe future disassembly
- repair and preservation work
- reverse engineering of internal architecture
- sidecar or deeper controller experiments

## What The New Teardown Guides Add

The teardown guides in `teardown/` are immediately useful because they capture
procedural knowledge that the Sony documents do not provide.

Files:

- [teardown/ERS-210 Body Shell Disassembly .pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Body%20Shell%20Disassembly%20.pdf)
- [teardown/ERS-210 Head Shell Disassembly .pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Head%20Shell%20Disassembly%20.pdf)
- [teardown/ERS-210 Tail Module Disassembly.pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Tail%20Module%20Disassembly.pdf)

These guides contribute:

- shell-opening order
- hidden screw locations
- clip-release techniques
- warnings about brittle plastics
- notes on warped or shrinking plastics
- notes on microphone screws and aged rubber gasket behavior
- tail module opening order and potentiometer access
- the practical warning that some head operations need more force than expected

That is highly valuable field knowledge.

## Strategy Overview

We should build our manual in three layers:

### Layer 1: Source preservation

Keep the raw teardown PDFs and original Sony documents in the repo unchanged.

Purpose:

- preserve provenance
- avoid losing original phrasing and diagrams
- let later work cite the exact source

### Layer 2: Structured internal notes

For each subsystem, create a repo-native markdown note that extracts:

- required tools
- prerequisites
- step order
- hidden fasteners
- fragile points
- connector and cable warnings
- photos and observations from our own robot

Purpose:

- make the knowledge searchable
- separate reusable facts from long PDF page sequences

### Layer 3: Evidence-backed house manual

After enough subsystem notes exist, consolidate them into a top-level
house manual that describes:

- disassembly order
- safe stopping points
- reassembly checkpoints
- known revision differences
- damage risks
- reporting format

Purpose:

- create a reliable working manual for this specific project

## Proposed Documentation Structure

I recommend this structure under `docs/`:

- `docs/disassembly-and-reporting-strategy.md`
- `docs/disassembly-body-shell.md`
- `docs/disassembly-head-shell.md`
- `docs/disassembly-tail-module.md`
- `docs/disassembly-leg-modules.md`
- `docs/disassembly-back-sensor-and-tail-skin.md`
- `docs/internal-connectors-and-cables.md`
- `docs/fastener-map.md`
- `docs/reassembly-checkpoints.md`
- `docs/observed-revision-differences.md`
- `docs/ers210-house-manual.md`

The teardown PDFs remain in `teardown/` as reference material.

## Recommended Working Method

### 1. Disassemble one boundary at a time

Do not perform a full uncontrolled strip-down in one session.

Instead:

- choose one subsystem boundary
- read all relevant notes first
- document before, during, and after
- stop at the first stable checkpoint

Good early boundaries:

- body shell only
- head shell only
- tail module only

### 2. Treat every hidden fastener as a reporting item

Whenever a step depends on:

- a hidden screw
- a hidden clip
- a wire routed through a narrow gap
- a part that requires force

it should become a named callout in markdown.

That is exactly the kind of knowledge people forget first.

### 3. Photograph every state transition

For every subsystem, capture:

- assembled exterior
- first access point opened
- every cable exposed
- every fastener removed
- every connector before unplugging
- every part after separation
- any damage, stress marks, or aging

If possible, standardize:

- top-down shot
- left side
- right side
- close-up of connector or hidden clip
- labelled bag or tray with screws

### 4. Separate observation from inference

In the notes, explicitly distinguish:

- `Observed`
- `Inferred`
- `Needs verification`

Example:

- `Observed`: hidden screw under ear hub anchors head to body
- `Inferred`: head wiring path may constrain full head-shell separation
- `Needs verification`: whether all ERS-210 revisions route the cable bundle identically

This prevents lore from turning into false certainty.

## Reporting Template

Every subsystem note should use the same format.

Recommended template:

### Title

Subsystem name and scope.

### Goal

What this procedure exposes or removes.

### Sources

- Sony docs used
- teardown PDFs used
- our own photos or sessions

### Tools

- screwdriver type
- prying tools
- tweezers
- tape
- trays or pill cases

### Preconditions

- battery removed
- module ejected
- workspace padded
- photos started

### Risk level

- low
- medium
- high

### Known fragile points

- brittle clips
- warped shells
- aged adhesive
- liquid or degrading rubber

### Step sequence

Numbered, concise, with one action per step.

### Hidden fasteners or catches

Every hidden or non-obvious retention feature listed plainly.

### Cable and connector notes

- where wires route
- whether unplugging is required
- whether connectors are risky

### Safe stopping point

State where the subsystem can be paused and stored safely.

### Reassembly notes

- alignment issues
- order dependencies
- screw-length hazards

### Evidence

- local image links
- observations from this robot

### Open questions

- unknown board markings
- unidentified cable
- revision uncertainty

## Physical Handling Rules

These should become the house rules for all future disassembly sessions:

- remove battery or battery emulator before any shell work
- never force a panel until hidden screws are ruled out
- bag and label screws by step, not by guess
- photograph each connector before touching it
- do not rely on memory for wire routing
- stop when force rises unexpectedly
- treat every old rubber part as suspect
- assume aged plastic may crack even if the same step worked on another robot

## Evidence We Should Capture For Reverse Engineering

The manual should not only explain shell removal. It should also collect data
useful for later architecture work.

At each opening stage, record:

- board identifiers
- connector counts and shapes
- cable bundle directions
- markings on flex cables
- visible regulators or power chips
- any buses or headers labeled on boards
- potentiometer and motor arrangement
- any revisions or shielding differences

This lets the disassembly work feed directly into the `GA144` feasibility
checklist.

## Recommended Phase Order

### Phase 1: Normalize the teardown knowledge

Create markdown extraction notes from the three current teardown PDFs:

- body shell
- head shell
- tail module

### Phase 2: First controlled documentation session

Use the shell notes on the real robot and record:

- what matched the PDFs
- what differed
- what was harder than described
- what new hidden details appeared

### Phase 3: Build the missing subsystem notes

After first-hand sessions, add:

- leg module note
- back sensor note
- connector note
- fastener map

### Phase 4: Consolidate into house manual

Create:

- `docs/ers210-house-manual.md`

This becomes the curated, project-specific disassembly manual.

## Immediate Recommendation

The best next action is:

1. convert each teardown PDF into a markdown subsystem note
2. add a standard reporting template
3. do the first controlled shell-only session on the actual robot
4. update the notes from direct evidence

That process will build a manual that is better than either:

- consumer Sony docs alone
- community teardown PDFs alone

because it will be both source-backed and specific to this robot and this
project.
