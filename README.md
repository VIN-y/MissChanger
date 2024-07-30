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

## Version Status

| Version | Status                                                                                                                                                | Remarks                                                                                                                                                                      |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| v1.0    | **Beta**<br/><br/>Detail:<br/>- The design is frozen.<br/>- Plug-in is functional.<br/>- config samples are published.<br/>- Documentation published. | **MVP** (Minimum Viable Product)<br/><br/>The purpose of the v1.x group is to provide the option with the bare minimum hardware required to have the MissChanger up and run. |
| v0.0    | Abandoned                                                                                                                                             | The design has been proven to lack durability in the testing phase.                                                                                                          |

## Compatibility

#### Printers:

- Voron 2.4 printer

#### Toolheads:

* Stealth Burner toolhead (up to 5, for the 350mm version)

#### Hotends:

* Revo-Voron
* Phaetus Dragon

## Assembly

BOM, STLs, and instructions for each version of are in their associated sub-folder in the [STLs and Instructions](./STLs%20and%20Instructions) folder.

## Software

The plugin for MissChanger is a fork of [klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger), for [Tapchanger](https://github.com/viesturz/tapchanger), and it is available via GitHub at [VIN-y/klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger). Installation and configuration steps are outlined in the [STLs and Instructions](./STLs%20and%20Instructions) folder.

Sample config files and their descriptions are available in the [Software](./Software) folder. MissChanger deviate greatly from the design of Tapchanger and Draftshift (Stealthchanger); thus, it's config files are not compatible upstream.

Other recommended software:

* [KIAUH](https://github.com/dw-0/kiauh) - For the purpose of managing Klipper updates, to handle any potential incompatibility issues, whenever there is a major Klipper update.

## Other information

### Offset Types

There are 2 type of offsets for each tool-head:

- **x_offset** / **y_offset** / **z_offset** - which are the default offsets that most people are used to, which will be referred to as probe offset moving forward. They are sort of like hard-coded values (they are not); in that, they are pretty hard to work with downstream, i.e. in the configs

- **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** - these are like for on-the-fly adjustment to for the gcode. This the stuff you adjust when you do baby-stepping mid-print

For the purpose of the tool-changer:

- **x_offset** and **y_offset** will not be used

- **z_offset** is to be calibrated for all toolheads

- **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** are used to account for the XYZ different between the nozzles, based on a reference nozzle

## Support

If you have any question, you can reach me:

* `@vin` in the [Voron Toolchangers](https://discord.gg/Gt5XCCwv) discord
