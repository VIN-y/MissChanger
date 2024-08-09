# MissChanger

A Stealth Burner tool-change system for Voron 2.4 and Trident.

* This project was inspired by [Stealthchanger](https://github.com/Stealthchanger/Toolchanger) and [TapChanger](https://github.com/viesturz/tapchanger/) 
- The Center_Tap for the Tap&Change system is from [Voron-Tap](https://github.com/VoronDesign/Voron-Tap/) 

- The endstops assembly is a remix of that from [MrTeliP](https://www.printables.com/model/325765-voron-24r2-pg7-cable-gland-and-endstop) 

- The exhaust cover is a remix of that from [Fiction](https://github.com/VoronDesign/VoronUsers/tree/main/printer_mods/Fiction/Exhaust_cover) 

- The Nudge calibration switch is from [zruncho3d](https://github.com/zruncho3d/nudge) 

![20240223_185152.jpg](./images/20240609_222649.jpg)

## Description

MissChanger aims to be a tool-changing upgrade that is compatible with both Voron Trident and Voron 2.4 (Trident compatibility is pending). While also retaining the MGN9H tapping system of Voron-Tap. In addition, MissChanger is designed with the ability to quickly and toollessly convert between tool changer mode and single nozzle mode (for when you need the full print volume of the stock printer).

## Compatibility

#### Printers:

- Voron 2.4 printer

#### Toolheads:

- Stealth Burner toolhead (up to 5, for the 350mm version)

#### Hotends:

- Revo-Voron
- Phaetus Dragon

## Assembly

BOM, STLs, and instructions for each version of are in their associated sub-folder in the [STLs and Instructions](./STLs%20and%20Instructions) folder.

## Software

The plugin for MissChanger is a fork of [klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger), for [Tapchanger](https://github.com/viesturz/tapchanger), and it is available via GitHub at [VIN-y/klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger). Installation and configuration steps are outlined in the [STLs and Instructions](./STLs%20and%20Instructions) folder.

Sample config files and their descriptions are available in the [Software](./Software) folder. MissChanger deviate greatly from the design of Tapchanger and Draftshift (Stealthchanger); thus, it's config files are not compatible upstream.

Other recommended software:

- [KIAUH](https://github.com/dw-0/kiauh) - For the purpose of managing Klipper updates, to handle any potential incompatibility issues, whenever there is a major Klipper update.

## Status and Roadmap

### Status definition

| Terms     |                                                                                                                                                                                                                                                                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Release   | Finalised.<br/>All relevant components are finished and tested.                                                                                                                                                                                                                                                                  |
| Beta      | All relevant components are locked in and packaged, see [Releases](./Releases). However, no user tests or feedback has been done.<br/>Note: Depending on the feedback from the beta, the relevant version might be updated into the release version. However, if significant changes are required, another version will be made. |
| Alpha     | Functionally working.<br/>CAD designs are locked in (frozen) but with no (or incomplete) documentations.                                                                                                                                                                                                                         |
| Bleeding  | Everything are subjected to changes.                                                                                                                                                                                                                                                                                             |
| Abandoned | Old design that is no longer persuaded.                                                                                                                                                                                                                                                                                          |

### Versions

| Version | Status    | Remarks                                                                          |
| ------- | --------- | -------------------------------------------------------------------------------- |
| v0.0    | Abandoned | The design has been proven to lack durability in the testing phase.              |
| v1.0    | Beta 1    | Although functional, several reliability and usability problems have been found. |
| v1.1    | Bleeding  | Fixes problems found in v1.0.                                                    |

##### v0.0 - Original idea

This was the original idea for MissChanger, which was developed up to the point where it can be used for test prints. It was discovered that the mechanism would wears down after ~2000 cycles and lost the precision needed for Quad Gantry Level.

No further work will be done for this design, and documentations are not available.

##### v1.[series] - MVP (Minimum Viable Product)

The purpose of v1.x series is to provide the option with the bare minimum hardware required to have the MissChanger up and run. This series is for those who are price conscious, as the most expensive part of the build will be the toolheads themselves.

Nevertheless, being designed as the MVP results in some fundamental flaws across the designs of the v1.x series, which will be addressed in v2.x series. These flaws are:

* Loss of z build volume (at around 200-250mm maximum z) - due to tangled umbilicals

* Lack of space between the toolhead and the front doors - Causing them to collide whenever there is a tool-change

* Low compatibility with user mods (i.e. idler mods)

###### v1.0 - Beta 1

This is the first version of MissChanger for others to pick-up and build upon. Through testing, it shows the following pros:

* The tool-change mechanism is reliable (with ~5000 cycles on T0, thus far)

* The stock front z-motors can carry the weight of the dock and 4 toolheads, ~2.5 kg

* The safety mechanism functions as intended, where the dock's Bar Ends will flex, before the dock are pushed into misalignment, before the front z-motors skip steps, before any damage is done. - This is in the event that the front gantry tilts down too far, causing the idlers to crash into the bottom corners of the dock.

* The umbilicals does not get tangled until at least Z200, and printer is still functional up to Z250 with some limitation around the edge of the build area.

* The system can be toggled between single toolhead and multi-toolheads mode without the use of any tool, in about 20-60s.

* Tool-change speed is relatively fast, ~8s

* Probe accuracy is very high, with `samples_tolerance: 0.00275` on all toolhead

* The Nudge calibration probe is very accurate and reliable

Nevertheless, the tests have also reveals the following cons:

* Tap&Change pieces are too fragile and would crack in the event of toolhead crash or with lower tolerance printed parts

* Incompatibility with GE5C bearing z mount for the Voron 2.4

* Incompatibility with front idler mods

* Tolerance for front gantry tilt is still too low

* Friction mounted toolheads are susceptible to be bumped loose by door closing

* There are limits to the material combination in a tool changer, due to the shared chamber and bed temperatures

* The PTFE tube influences the curve of the umbilical more than everything else, except the top panel of the printer

* Input shaper result is worse than that of the original Voron Tap

###### v1.1 - Bleeding

* Adds magnets to the docking system to hold onto the toolhead more securely when docked

* Increase wall thickness of the Tap&Change pieces

* Migrate the project files to Ondsel, the CAD software

### Roadmap

#### v1.[series]

- [ ]  Lock in the Tap&Change and dock design for all future versions

- [ ]  Trident compatibility (i.e. additional calibration probe mount design)

- [ ]  Print capability test, material combination

#### v2.[series]

- [ ]  Regain lost z build volume - with a top hat

- [ ]   Increase space between the toolhead and the front doors and compatibility with user mods - with a new dock that integrates into a "fridge door" of 20x20mm aluminium extrusion 

## Other information

### Offset Types

There are 2 type of offsets for each tool-head:

- **x_offset** / **y_offset** / **z_offset** - which are the default offsets that most people are used to, which will be referred to as probe offset moving forward. They are sort of like hard-coded values (they are not); in that, they are pretty hard to work with downstream, i.e. in the configs

- **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** - these are like for on-the-fly adjustment to for the gcode. This the stuff you adjust when you do baby-stepping mid-print

For the purpose of the tool-changer:

- **x_offset** and **y_offset** will not be used

- **z_offset** is to be calibrated for all toolheads

- **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** are used to account for the XYZ different between the nozzles, based on a reference nozzle

## Recommendations

1. Keep toolheads as similar as possible. - The more variations there are between toolheads (i.e. control board, hotend system, etc.), the more tuning will be need for each of them.

## Support

If you have any question, you can reach me:

* `@vin` in the [Voron Toolchangers](https://discord.gg/Gt5XCCwv) discord
