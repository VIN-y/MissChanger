# MissChanger

A Stealth Burner tool-change system for Voron 2.4 and Trident.

* This project was inspired by [Stealthchanger](https://github.com/Stealthchanger/Toolchanger) and [TapChanger](https://github.com/viesturz/tapchanger/) 
- The Center_Tap for the Tap&Change system is from [Voron-Tap](https://github.com/VoronDesign/Voron-Tap/) 

- The end-stops assembly is a remix of that from [MrTeliP](https://www.printables.com/model/325765-voron-24r2-pg7-cable-gland-and-endstop) 

- The exhaust cover is a remix of that from [Fiction](https://github.com/VoronDesign/VoronUsers/tree/main/printer_mods/Fiction/Exhaust_cover) 

- The Nudge calibration switch is from [zruncho3d](https://github.com/zruncho3d/nudge) 

![20240223_185152.jpg](./images/20240921_201730.jpg)

## Description

MissChanger aims to be a tool-changing upgrade that is compatible with both Voron Trident and Voron 2.4 (Trident compatibility is pending). While also retaining the MGN9H tapping system of Voron-Tap. In addition, MissChanger is designed with the ability to quickly and toollessly convert between tool changer mode and single nozzle mode (for when you need the full print volume of the stock printer).

## Compatibility

#### Printers:

- Voron 2.4 printer 350

#### Toolheads:

- Stealth Burner (up to 5, for the 350mm version) 

#### Hotends:

Any hot-ends that does not require a custom, extended main-body for the Stealth Burner should works just fine. Nevertheless, the follows are those that has been tested on the reference machine:

- Revo-Voron
- Phaetus Dragon - Not recommended, due to the large heater block causing slow heat-up and cool-down. This is not ideal, since it is beneficial to lower the tool temperature when it is docked for a long period of time.

## Assembly

BOM, STLs, and instructions for each version of are in their associated sub-folder in the [STL_Config_Instructions](./STL_Config_Instructions) folder.

## Software

The plugin for MissChanger is a fork of [klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger), for [Tapchanger](https://github.com/viesturz/tapchanger), and it is available via GitHub at [VIN-y/klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger). Installation and configuration steps are outlined in the [STL_Config_Instructions](./STL_Config_Instructions) folder. Sample config files and their descriptions are also available in the folder. 

MissChanger deviate greatly from the design of Tapchanger and Draftshift (Stealthchanger); thus, it's config files are not compatible upstream.

Other recommended software:

- [KIAUH](https://github.com/dw-0/kiauh) - For the purpose of managing Klipper updates, to handle any potential incompatibility issues, whenever there is a major Klipper update.

## Roadmap

### v1

- [x] ~~Lock in the Tap&Change and dock design for all future versions~~ - v1.1. is the final version for the Tap&Change system for the Stealth Burner

- [x] ~~Print capability test, material combination~~ - Tested with the three most common materials

- [ ] Compatibility with smaller Vorons

- [ ] USB tool-head support

- [ ] Trident compatibility

### v2

- [ ] Support for additional tool-heads

- [ ] Support for different type of probe

## Other information

### Status definition

| Terms     | Description                                                                                  |
| --------- | -------------------------------------------------------------------------------------------- |
| Release   | Finalised.<br>All relevant components and documentations are finished and tested.            |
| Beta      | All relevant components are locked in.<br/>However, without substantial testing or feedback. |
| Alpha     | CAD designs locked in.<br>No (or incomplete) documentations.                                 |
| Bleeding  | Everything are subjected to changes.                                                         |
| Abandoned | Old design that is no longer being persuaded.                                                |

Note: 

* The .step and CAD project files for all but the most recent version will not be included in this repository, due to the storage/transfer limits of GitHub.

* With user feedback and further testing, the relevant version might be updated to the next tier. However, if significant changes are required, another version will be made and start at the bleeding state; and the current version will no longer be persuaded (i.e. abandoned).

### Offset Types

There are 2 type of offsets for each tool-head:

- **x_offset** / **y_offset** / **z_offset** - which are the default offsets that most people are used to, which will be referred to as probe offset moving forward. They are sort of like hard-coded values (they are not); in that, they are pretty hard to work with downstream, i.e. in the configs

- **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** - these are like for on-the-fly adjustment to for the gcode. This the stuff you adjust when you do baby-stepping mid-print

For the purpose of the tool-changer:

- **x_offset** and **y_offset** will not be used

- **z_offset** is to be calibrated for all tool-heads

- **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** are used to account for the XYZ different between the nozzles, based on a reference nozzle

### Build area set up

<mark>This is **an important point** to keep in mind.</mark> It is important to know how your build area is set up, to avoid collisions and potential damages.

MissChanger use the same build area configuration as the origin Voron 350mm, see the code block below. This build area set up stay the same between the single tool-head  and the multi tool-heads mode, providing consistency. No matter which mode you are in, `G1 X{number} Y{number}`  will bring the gantry to the same spot above the build plate. Also, the creator's (that is me) finds negative coordinates confusing.

```
[stepper_x]
...
position_min: 0               # X-axis minimum travel - software limit
position_endstop: 350         # Mechanical reset point coordinates for X-axis (change to 350 for 350 models)
position_max: 350             # X-axis maximum travel - software limit (change to 350 for 350 models)
...

[stepper_y]
...
position_min: 0               # Y-axis minimum travel - software limit
position_endstop: 350         # Mechanical reset point coordinates for Y-axis (change to 350 for 350 models)
position_max: 350             # Y-axis maximum travel - software limit (change to 350 for 350 models)
...
```

Nevertheless, this behaviour can be change in the config, by bring the settings for `[stepper_x]` and `[stepper_y]` under the `Session Variables` section.

## Recommendations

1. Keep tool-heads as similar as possible. - The more variations there are between tool-heads (i.e. control board, hot-end system, etc.), the more tuning will be need for each of them.

2. The following mods are not needed but is recommended to enhance the capability of MissChanger:
   
   1. [Calilantern Calibration Tool](https://vector3d.shop/products/calilantern-calibration) - MissChanger requires relatively high dimensional-accurate printed parts, to mate with each others. Therefore, it is recommended that you take the extra steps to calibrate your printer before starting the build.
   2. [Nevermore StealthMax](https://github.com/nevermore3d/StealthMax) - to regulate the chamber temperature, to allow PETG / PLA printing with the door closed; allowing them to be combined with ABS.
   3. [Spool Holder](https://www.printables.com/model/872144-spool-holder-for-2020-aluminium-extrusion) - to hold the spools on top of the machine. There are many ways to get this done, and it depends on the people's specific use-cases. The design linked here is just of my (the developer) own creation.
   4. [Inverted z-chain](https://www.printables.com/model/445298-inverted-z-chain-for-voron) - This is for the Voron 2.4 300mm, or smaller. To clear the space in front of the back gantry extrusion for the umbilicals.

3. The following mods are either not recommended or known to be not compatible with MissChanger:
   
   1. GE5C bearing z mount - This mod offer too much flexibility to the gantry, allowing it to sag when the z motors are disabled/un-powered.
   
   2. Beefy front idlers - The cut off on the dock can only fit the stock front idlers.
   
   3. AWD - The cut off on the dock can only fit the stock front idlers.

## Support

If you have any question, you can reach me:

* `@vin` in the [Voron Toolchangers](https://discord.gg/qrb6HXNYxs) discord
* `@vinny_69 V2.6365` in the [VORONDesign](https://discord.gg/voron) discord
