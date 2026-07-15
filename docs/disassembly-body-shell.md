# ERS-210 Body Shell Disassembly

## Goal

Remove the ERS-210 body shell plastics and related outer coverings without
opening deeper internal electronics beyond what shell removal exposes.

This note is intended as the first repo-native subsystem procedure derived from
the teardown material and aligned with the reporting strategy for building an
ERS-210 house manual.

## Sources

- [teardown/ERS-210 Body Shell Disassembly .pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/teardown/ERS-210%20Body%20Shell%20Disassembly%20.pdf)
- [docs/ModelInformation_210_E.pdf](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/ModelInformation_210_E.pdf)
- [docs/disassembly-and-reporting-strategy.md](/home/cartheur/ame/aiventure/aiventure-github/cartheur-aibo/ERS-210/docs/disassembly-and-reporting-strategy.md)

## Scope

This procedure covers:

- front leg shell plastics
- rear leg shell plastics
- hip and body shell plastics attached to those leg assemblies
- butt bumpers
- back sensor shell and silicone tail skin

This procedure does not yet cover:

- head shell
- tail gearbox internals
- board removal
- connector mapping
- full reassembly validation

## Tools

- `JIS 0` or `JIS 00` screwdriver
- fingernails or a non-marring prying tool
- labelled screw organizer or pill case
- masking tape for temporary labeling if needed
- padded work surface

## Preconditions

- battery removed
- `Memory Stick` removed if appropriate for the session
- all limb modules ejected from the body core before starting
- photo log started
- screw organizer prepared and labelled by step or subsystem

## Risk Level

- `Medium`

The shell work is mechanically straightforward, but the aging plastics may be
brittle and slightly warped.

## Known Fragile Points

- colored shell plastics may be brittle from age
- black and colored plastics may cling due to clip tension and shrinkage
- screw hole alignment may be worse on reassembly due to warping
- butt bumpers have adhesive underneath
- hidden screws in servo-access holes are easy to miss
- clips can require more force than feels comfortable

## Relevant Model Facts

The Sony model-information document confirms:

- the body includes repeated leg modules
- the robot has a tail block and back sensor region
- the body contains both `Memory Stick` and `PC Card` access internally
- each leg is a 3-joint module

Those facts do not define disassembly order by themselves, but they support the
idea that shell removal should be treated in modular zones.

## Step Sequence

### 1. Prepare and organize

1. Eject all limb modules from the body core before starting shell work.
2. Place the robot and removed modules on a padded surface.
3. Prepare a labelled screw organizer for:
   - front lower leg screws
   - front upper leg screws
   - front hip/body screws
   - rear lower leg screws
   - rear upper leg screws
   - rear hip/body screws
   - back sensor screws

### 2. Front lower leg shell

1. Start with a front lower leg.
2. Remove the two screws on the back of the lower leg shell.
3. Pry between the colored and black plastics to release the clips.
4. If clips resist release, gently squeeze the plastic sides while prying.
5. Pull the plastic sides outward.
6. Push the top part of the leg plastic forward.
7. Flex the lower shell wider and guide it forward over the foot plastics.

### 3. Front upper leg shell

1. Remove the three upper leg shell screws.
2. Keep track of screw sizes carefully.
3. The bottom screw is smaller than the two upper screws.
4. Pry the black and colored plastics apart.
5. Pull each side away.
6. Rotate the leg position if needed to free the top part of the black plastic.
7. Pull the colored plastic away from the leg.

### 4. Front hip and body shell section

1. Flip the leg over.
2. Remove all silver screws associated with the front hip/body shell section.
3. Do not miss the hidden screw deep inside the hole where the servo sits.
4. Once all screws are out, separate the hip/body shell section carefully.
5. Set the leg aside in a stable orientation so the hip layers are not stressed.

### 5. Rear lower leg shell

1. Move to a rear leg.
2. Remove the two lower leg screws.
3. Note that the bottom screw is smaller than the top screw.
4. Pull the colored plastic upward and off, guiding the leg through the middle gap.
5. Remove the black backing plastic.

### 6. Rear upper leg shell

1. Start prying at the bottom near the angled area and work upward.
2. This lower clip is reported as the hardest clip, so expect resistance here.
3. Move the top part of the colored plastic forward.
4. Remove the three upper leg screws.
5. Pull the black plastic away from the colored plastic.
6. Move the leg if needed to unhook the upper black plastic.
7. Pull the colored plastic off the leg.
8. Pull the bottom corners outward and push the shell forward over the paw plastic.

### 7. Rear hip and body shell section

1. Remove all silver screws from the rear hip/body shell section.
2. Include the hidden screw in the hole.
3. These rear hip screws are reported as the same type, so they may be grouped.
4. Lead the leg through the hole in the leg plastic as the shell section separates.

### 8. Butt bumpers

1. If bumper removal is needed, locate the two tabs on the inside of the bumper.
2. Push the tabs through until they begin lifting out of the plastic.
3. Expect adhesive tape underneath the bumper.
4. Do not mistake adhesive sounds for cracking.

### 9. Back sensor and silicone tail skin

1. Pull the silicone tail skin off.
2. Flip the back sensor assembly over.
3. Remove the two screws.
4. Lift the colored plastic up off the black piece.
5. If needed, lift the touch panel from the edge and peel it up around the perimeter.

## Hidden Fasteners And Catches

- front hip/body shell has a hidden screw deep in the servo hole
- rear hip/body shell has a hidden screw deep in the corresponding hole
- lower leg clips may require squeezing and prying together
- rear lower clip near the angled area is especially stubborn
- butt bumpers are retained by internal tabs and adhesive

## Cable And Connector Notes

- This teardown note mostly concerns shells, not electrical disconnects
- No mandatory connector unplugging is described in the body shell guide
- Treat any unexpectedly exposed cable bundle as a stop-and-document moment
- If electrical routing becomes visible during a live session, photograph before touching

## Safe Stopping Points

Good stopping points for a session:

- after one front leg shell is fully documented
- after both front hip/body shell sections are removed
- after both rear hip/body shell sections are removed
- after back sensor shell removal

Do not stop mid-pry with a stressed clip if it can be safely resolved first.

## Reassembly Notes

- some shell parts may resist clipping back together due to age-related shrinkage or warping
- screws may not line up cleanly on first attempt
- squeezing and pushing the shells together may be required during reassembly
- keep paint off clip interfaces and mating edges if refinishing parts
- do not paint inside shell surfaces if future fitment matters

## Reporting Requirements For First Live Session

During our own first body-shell session, record:

- exact screw counts removed from each front leg zone
- exact screw counts removed from each rear leg zone
- whether screw lengths match the teardown guide’s claims
- which clips were hardest to release
- any cracks, whitening, or stress marks
- any visible internal board, harness, or connector markings after shell removal
- whether hidden screws are identical front to rear

## Evidence To Capture

At minimum, capture photos of:

- each leg before starting
- front lower leg rear screw locations
- front upper leg three-screw pattern
- front hidden screw in servo hole
- rear lower leg clip and shell opening order
- rear upper shell three-screw pattern
- rear hidden screw in servo hole
- butt bumper retention tabs
- back sensor shell front and rear

## Observed

- The teardown source strongly emphasizes brittle plastics and patience.
- Front upper leg uses a smaller bottom screw than the two upper screws.
- Hidden screws in servo-access holes are easy to overlook.
- Rear lower clip near the angled section is unusually resistant.
- Butt bumpers include adhesive as well as tab retention.
- Back sensor shell appears mechanically simple compared with the limb shells.

## Inferred

- Front and rear shell work should be documented as separate procedures even if they look similar.
- Hidden fastener tracking will likely be one of the most important differences between a safe and unsafe session.
- A screw map should eventually be promoted into its own repo document.

## Needs Verification

- exact front versus rear screw length table
- whether all ERS-210 revisions share the same shell clip geometry
- whether shell shrinkage severity correlates with colorway or storage history
- whether any shell sections expose internal connector routes that should be recorded in a separate note

## Next Recommended Document

After using this note once on the real robot, the best next companion note is:

- `docs/fastener-map.md`

That note should collect:

- screw counts
- screw lengths
- screw head types
- subsystem ownership
- hidden fastener locations
