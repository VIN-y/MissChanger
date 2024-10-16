# Introduction

This folder contains the STLs, config files, and instructions for both the latest and past version of MissChanger.

Each versions are enclosed in their own folder. These folder should contains all the information that is available and relevant to that version of the system. Nevertheless, this directory structure was not fully fleshed out until v1.1. Therefore, the documentation for versions that are older than v1.1. might not be functional.

# Design Record

These folder contains all the .step files that are produced for this project.

## v1 - MVP (Minimum Viable Product)

The purpose of **v1** is to provide the option with the bare minimum hardware required to have the MissChanger up and run. This series is for those who are price conscious or do not need a dedicated tool-changer all the time. The most expensive part of a v1 build will be the toolheads themselves.

## v2 - Tool-changer focused design

The purpose of **v2** is to overcomes the inherent flaws of **v1** noted above. However, this will be increase the price of the system, with more components added.

## Versions Summary

| Version | Status    | Remarks                                                                                                                                                               |
| ------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| v0.0    | Abandoned | The design has been proven to lack durability in the testing phase.                                                                                                   |
| v1.0    | Abandoned | Although functional, several reliability and usability problems have been found.                                                                                      |
| v1.1    | Alpha     | Fixes problems found in v1.0.<br/>Tool-changer reliability proven and will be carried forward, unless stated otherwise.<br/>Chamber temperature management is needed. |
|         |           |                                                                                                                                                                       |

## Design History

### v0.0 - Original idea

This was the original idea for MissChanger, which was developed up to the point where it can be used for test prints. It was discovered that the mechanism would wears down after ~2000 cycles and lost the precision needed for Quad Gantry Level.

No further work will be done for this design, and documentations are not available.

### v1.0 - First Beta

This is the first version of MissChanger for others to pick-up and build upon. Through testing, it shows the following pros:

- The tool-change mechanism is reliable (with ~5000 cycles on T0, thus far)

- The stock front z-motors can carry the weight of the dock and 4 toolheads, ~2.5 kg

- The safety mechanism functions as intended, where the dock's Bar Ends will flex, before the dock are pushed into misalignment, before the front z-motors skip steps, before any damage is done. - This is in the event that the front gantry tilts down too far, causing the idlers to crash into the bottom corners of the dock.

- The umbilicals does not get tangled until at least Z200, and printer is still functional up to Z250 with some limitation around the edge of the build area.

- The system can be toggled between single toolhead and multi-toolheads mode without the use of any tool, in about 20-60s.

- Tool-change speed is relatively fast, ~8s

- Probe accuracy is very high, with `samples_tolerance: 0.00275` on all toolhead

- The Nudge calibration probe is very accurate and reliable

Nevertheless, the tests have also reveals the following cons:

- Tap&Change pieces are too fragile and would crack in the event of toolhead crash or with lower tolerance printed parts

- Incompatibility with GE5C bearing z mount for the Voron 2.4

- Incompatibility with front idler mods

- Tolerance for front gantry tilt is still too low

- Friction mounted toolheads are susceptible to be bumped loose by door closing

- There are limits to the material combination in a tool changer, due to the shared chamber and bed temperatures

- The PTFE tube influences the curve of the umbilical more than everything else, beside the top panel

- Input shaper result is worse than that of the original Voron Tap

### v1.1 - Fixing problems with v1.0 and further testings

- Migrate the project files to Ondsel, the CAD software, and tighten up part-to-part tolerances

- Adds magnets to the docking system to hold onto the toolhead more securely when docked

- Increase wall thickness of the Tap&Change pieces, making it more durable

- Add door buffer, for clearance of the docked toolheads.

- Add silicon liner layer for nozzle plug, to avoid PETG-caused damage.

- Passed reliability test with 4100 continuous toolchanges at random z-positions

- Adds support for the Nevermore StealthMax, with a modified flow chamber with output switch.

- User feedback. A design flaw has been discovered in the XLR Panels, which cause slight incompatibility with smaller Voron.

- Testing has shown that the better air flow control can significantly increase the ultilisation of the bed heater for chamber heating.

- Multi-material experiments show that the toolchanger will benefit from chamber temperature management. Lower chamber temperature will allow ABS to be combined with PLA/PETG.
