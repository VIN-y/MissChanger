# Introduction

This folder contains the STLs, config files, and instructions for both the latest and past version of MissChanger.

Each versions are enclosed in their own folder in the [MissChanger Archive](https://drive.proton.me/urls/3CDH12JTMW#YhJ5K3t4AadW). These folder should contains all the information that is available and relevant to that version of the system. Nevertheless, this directory structure was not fully fleshed out until v1.1, and CAD models reservation were not properly done until v1.2. Therefore, the documentation for versions that are older than v1.1 might not be functional, and no CAD file older than v1.2. is available.

## Situational / Optional / Mods

These following attachments are extras that will expand the capability of tool-changer system. Nevertheless, they were developed by others and does not share the same design language as MissChanger (i.e. difference print parameters).

| Mod                                                                                                                            | Description                                                                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| [Nevermore Stealth Max](./6_Optionals/Nevermore_StealthMax)<br> + [DC Barrel Panel Mount](./6_Optionals/DC_Barrel_Panel_Mount) | - With modded flow chamber<br>- Cable management for Nevermore Stealth Max                                                    |
| [Galileo 2 + LDO Nitehawk SB USB cable strain relief](./5_Others/G2E-umbilical_anchor)                                         | For Galileo 2 + LDO Nitehawk SB USB                                                                                           |
| [Inverted z-chain](https://www.printables.com/model/445298-inverted-z-chain-for-voron)                                         | Recommended for the Voron 2.4 300mm, or smaller. To clear the space in front of the back gantry extrusion for the umbilicals. |

# Summary

| Version | Status    | Remarks                                                                                                                                                                                                                                              |
|:-------:|:---------:| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| v0.0    | Abandoned | The design has been proven to lack durability in the testing phase.                                                                                                                                                                                  |
| v1.0    | Beta      | Although functional, several reliability and usability problems were found.                                                                                                                                                                          |
| v1.1    | Alpha     | Fixes problems found in v1.0.<br/>Tool-changer reliability tested and proven. It will be carried forward, unless stated otherwise.<br/>The Nevermore StealthMax is added as optional chamber temperature management.                                 |
| v1.2    | Beta      | Add support for Voron 2.4 300mm.<br/>Add support for USB tool-heads.<br/>Add G2E official support.<br/>Revamp software stack and bug fixes.<br/>Preliminary Trident support.                                                                         |
| v1.2.1  | Beta      | Klipper compatibility fix.<br/>Move fan control to the the back-end, for smoother operation.<br/>Add more user controllable variables.<br/>Separate the `Klipper_Config`, to allows for software hot fix without the need to create another release. |
| v1.2.2  | Release   | Minor updates to CAD.<br/>Update documentations.                                                                                                                                                                                                     |

# Change Log

### v1.2.2 - minor update for Release 1

- Update Endstops Assembly for easier post-processing.

- Update documentation.

### v1.2.1 - Hot fix and add features

* Klipper compatibility fix.

* Move fan control to the the back-end, for smoother operation.

* Add more user controllable variables.

* Separate the `Klipper_Config`, to allows for software hot fix without the need to create another release.

### v1.2 - Polish hardware and revamp software

- Remigrating the project back to FreeCAD, because Ondsel ran out of funding and shutdown.

- Added official Nitehawk SB USB tool-head board.

- Voron 2.4 300 compatibility established, tested by `@psychosis5150`.

- Update the design of the Nudge Mount, for broader compatibility and increase durability.

- Update XLR panel design for better compatibility and alignment.

- Add official support for the G2E.

- Introducing the Lubedballs probe, as an alternative to the Nudge probe. Intended as an easier system to build.

- Fix some compatibility issue for Voron Trident.

- Bug fixes and revamp software stack:
  
  - Added changes from [DraftShift/klipper-toolchanger](https://github.com/DraftShift/klipper-toolchanger). This fork faster moving and has more features on top of [viesturz/klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger). Nevertheless, it is noted that Viesturz is still slowly adding features to his extension. Therefore, future updates to [VIN-y/klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger) (for MissChanger) may adopt features from either of the for mention repository, where they may fit for the project.
  
  - Deprecate `multi-fan` to re-enable the ability to control the part cooling fan with the GUIs.
  
  - Establish the structure of the software stack, to have a low barrier of entry while still allow customisation.
  
  - Revamp the tool-change routine, to avoid firmware crashing or unsafe movement during pick-up.
  
  - Add fail to drop-off detection.
  
  - Fixed the bug where the `gcode_z_offset` not correctly clear out after tool-change.
  
  - Revamped `CALIBRATE_OFFSETS` routine:
    
    - To check for proper probe placement before starting the session.
    
    - Add an option to run the routine with or without absolute z calibration. For over sensitive probes.
  
  - Add a macro to calibrate trigger bottom. So the printer don't need to be restarted and homed multiple times during calibration.
  
  - Fix the bug where the QGL failure is not detected in `PRINT_START`.
  
  - Fix compatibility issue with PrusaSlicer 2.9.0+, where the XY coordinate of the wipe tower is no longer get emitted.
  
  - Fix the bug with part cooling fan speed being stuck at random values midway through the print.
  
  - Add the function to save z_offset baby-steps.

### v1.1 - Fixing problems with v1.0 and further testings

- Migrate the project files to Ondsel, the CAD software, and tighten up part-to-part tolerances

- Adds magnets to the docking system to hold onto the tool-head more securely when docked

- Increase wall thickness of the Tap&Change pieces, making it more durable

- Add door buffer, for clearance of the docked tool-heads.

- Add silicon liner layer for nozzle plug, to avoid PETG-caused damage.

- Passed reliability test with 4100 continuous tool-changes at random z-positions

- Adds support for the Nevermore StealthMax, with a modified flow chamber with output switch.

- User feedback. A design flaw has been discovered in the XLR Panels, which cause slight incompatibility with smaller Voron.

- Testing has shown that the better air flow control can significantly increase the utilisation of the bed heater for chamber heating.

- Multi-material experiments show that the tool-changer will benefit from chamber temperature management. Lower chamber temperature will allow ABS to be combined with PLA / PETG. However, the result is mixed, as many ABS print came out poorly due to the lower chamber temperature.

### v1.0 - First Beta

This is the first version of MissChanger for others to pick-up and build upon. Through testing, it shows the following pros:

- The tool-change mechanism is reliable (with ~5000 cycles on T0, thus far)

- The stock front z-motors can carry the weight of the dock and 4 tool-heads, ~2.5 kg

- The safety mechanism functions as intended, where the dock's Bar Ends will flex, before the dock are pushed into misalignment, before the front z-motors skip steps, before any damage is done. - This is in the event that the front gantry tilts down too far, causing the idlers to crash into the bottom corners of the dock.

- The umbilical does not get tangled until at least Z200, and printer is still functional up to Z250 with some limitation around the edge of the build area.

- The system can be toggled between single tool-head and multi-tool-heads mode without the use of any tool, in about 20-60s.

- Tool-change speed is relatively fast, ~8s

- Probe accuracy is very high, with `samples_tolerance: 0.00275` on all tool-head

- The Nudge calibration probe is very accurate and reliable

Nevertheless, the tests have also reveals the following cons:

- Tap&Change pieces are too fragile and would crack in the event of tool-head crash or with lower tolerance printed parts

- Incompatibility with GE5C bearing z mount for the Voron 2.4

- Incompatibility with front idler mods

- Tolerance for front gantry tilt is still too low

- Friction mounted tool-heads are susceptible to be bumped loose by door closing

- There are limits to the material combination in a tool changer, due to the shared chamber and bed temperatures

- The PTFE tube influences the curve of the umbilical more than everything else, beside the top panel

- Input shaper result is worse than that of the original Voron Tap

- This version was quick to be pushed to beta, because there was only one MissChanger in the work. Future versions will be given more time at each tier, as more people get on board with the project

### v0.0 - Original idea

* This was the original idea for MissChanger, which was developed up to the point where it can be used for test prints. It was discovered that the mechanism would wears down after ~2000 cycles and lost the precision needed for Quad Gantry Level.
  
  No further work will be done for this design, and documentations are not available.
