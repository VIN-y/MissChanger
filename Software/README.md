# INSTRUCTIONS - config

This section aims to provide a guide through the installation process of the software and the config setup. The files included in this folder are meant to serve as sample configs, to be read, understood, and modified.

## Software

### [kiauh: Klipper Installation And Update Helper](https://github.com/dw-0/kiauh)

Beside the usual benefits of KIAUH; i.e.: update, rollback, etc. KIAUH also provides an extra (optional) extension called `[G-Code Shell Command]`, which will be needed for the printer mode toggle, between multi-toolheads and single-toolhead mode.

### [klipper-toolchanger: Toolcahnging extension for Klipper](https://github.com/viesturz/klipper-toolchanger)

This is the main extension for the tool changer. However, MissChanger deviate greatly from that of the original creator (Viesturz); thus, it's config are not compatible with other projects like Tapchanger and DraftShift. 

### Software Version

| Software            | Version     |
| ------------------- | ----------- |
| klipper             | v0.12.0-256 |
| klipper-toolchanger | TBD         |
| KIAUH               | TBD         |

## The basis

1. There is a default command for z-offset calibration, `PROBE_CALIBRATE`. It is part of default Klipper and is needed for the initial calibration

2. There are 2 type of offsets for each tool-head:
   
   - **x_offset** / **y_offset** / **z_offset** - which are the default offsets that most people are used to, which will be referred to as probe offset moving forward. They are sort of like hard-coded values (they are not); in that, they are pretty hard to work with downstream, i.e. in the configs
   
   - **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** - these are like the an on-the-fly adjustment to for the gcode. This the stuff you adjust when you do baby-stepping mid-print

3. For the purpose of the tool-changer:
   
   - **x_offset** and **y_offset** will not be used
   
   - **z_offset** is to be calibrates for all toolheads
   
   - **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** are used to account for the XYZ different between the nozzles, based on a reference nozzle

## Config files

This section aims to provide detail descriptions to each file included in this folder, which are need for MissChanger.

### calibrate-offsets.cfg

TBD

### homing.cfg

TBD
