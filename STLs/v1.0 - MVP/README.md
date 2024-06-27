# INSTRUCTION

## 1. Glossary

| Terminology  | Definitions                                                                                       |
|:------------ | ------------------------------------------------------------------------------------------------- |
| probe offset | The distance between the trigger point of the XYZ probes / endstops and their expected position.  |
| tool offset  | The nozzle position and the relative to the position of the nozzle of the reference toolhead, T0. |
| T0           | Reference toolhead                                                                                |
| G28          | this command to home all (XYZ).                                                                   |

## 2. Assembly Guide

### 2.1. Print parameters

All parts are designed to be print with the following parameters:

* All parts are pre-orientated in the STLs.

* 40% infill

* 4 walls

* ABS or ASA

### 2.2. Dock

**Dock BOM:**

| Quantity | Item                           |
| -------- | ------------------------------ |
| 10       | M3x16mm bolts                  |
| 4        | M3x8mm bolts                   |
| 1        | M3x10mm countersunk bolts      |
| 13       | M3x5x4mm threaded inserts      |
| 2        | M3 washers                     |
| 4        | M2x10mm self-tapping screw     |
| 2        | M2 washers                     |
| 4        | 3x20mm rounded pins            |
| 1        | 0.2mm oven liner cut-out       |
| 1        | 0.2mm feeler gauge (10mm wide) |
| 2        | 10x467mm solid aluminium rods  |
| 1        | Brass brush                    |

Note: the Dock BOM is only for a single dock. Additional docking bay will requires:

- (2 of) M3x16mm bolts

- (1 of) M3x10mm countersunk bolts

- (3 of) M3x5x4mm threaded inserts

- (2 of) M2x10mm self-tapping screw

- (2 of) M2 washers

- (4 of) 3x20mm rounded pins

- (1 of) 0.2mm oven liner cut-out

- (1 of) 0.2mm feeler gauge (10mm wide)

Follow this video guide for the dock assembly: [MissChanger - Build Guide - Dock](https://youtu.be/sSsay7bBFj0)

### 2.3. XLR Panels

### 2.4. Tool-head and Endstops Assembly

### 2.5. Calibration Probe

### 2.6. Wiring

In this section, it is assumed that you have already have a working print, i.e. with the controller board connected and working. Therefore, no instruction will be given for the wiring of the controller, SBC, Z-axis motors, etc.

#### CAN bus tool-head wiring diagram is as follow:

![](./images/MissChanger%20CAN%20wiring.jpg)

## 3. Hardware Alignment

TBD

## 4. Software Calibration

### 4.1. The basis

1. There is a default command for z-offset calibration, `PROBE_CALIBRATE`. It is part of default Klipper and is needed for the initial calibration

2. There are 2 type of offsets for each tool-head:
   
   - **x_offset** / **y_offset** / **z_offset** - which are the default offsets that most people are used to, which will be referred to as probe offset moving forward. They are sort of like hard-coded values (they are not); in that, they are pretty hard to work with downstream, i.e. in the configs
   
   - **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** - these are like the an on-the-fly adjustment to for the gcode. This the stuff you adjust when you do baby-stepping mid-print

3. For the purpose of the tool-changer:
   
   - **x_offset** and **y_offset** will not be used
   
   - **z_offset** is to be calibrates for all toolheads
   
   - **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** are used to account for the XYZ different between the nozzles, based on a reference nozzle

### 4.2. Calibrate Reference Toolhead

1. Mount toolhead T0

2. Go to T0  config file
   
   - Comment out the `z_offset` under `[tool_probe T0]` 
   
   - Set every other offsets to 0
   
   - Save it, <mark>**but don't restart**</mark>

3. Run `PROBE_CALIBRATE` on the console and go through the process with the normal paper test

4. Run `SAVE_CONFIG` - which will restart your printer

*Note: it is key that you get the z_offset correct for the T0, as it will be used to extrapolate other offsets later on. Therefore, it is worth diverge from the instruction, if you have a preferred way to set your the z-offset.*

### 4.3. Nudge Probe Calibration

*Note: It is worth mentions that there are other ways to calibrate your offsets beside the Nudge, such as the visual based solution from [Ember](https://www.emberprototypes.com/products/cxc) or the calibration print that you can get from [Printables](https://www.printables.com/model/201707-x-y-and-z-calibration-tool-for-idex-dual-extruder-). Each with their pros and cons, in term of accuracy and cost. However, Nudge is currently the only way to calibrate all relevant offsets.*

At this stage, you should have:

- The Nudge probe assembled, tested, and installed

- T0 z_offset calibrated

This section will guide you through the calibration of the `trigger_to_bottom_z` for the probe, which will allow you to automate the z_offsets of the toolheads that are not T0.

1. Go to **printer.cfg** and record the `z-offset` for `#*# [tool_probe T0]`, which should be at (or near) the bottom of the file

2. Download the [calibrate-offsets.cfg](https://github.com/VIN-y/MissChanger/blob/main/Software/Functions%20and%20Marcros/calibrate-offsets.cfg), add it into your config folder, and make the preference to it in the **printer.cfg** file:
   
   ```
   [include calibrate-offsets.cfg]
   ```

3. Open `calibrate-offsets.cfg`

4. Set it to the correct pin in `[tools_calibrate]`

5. Set the approximate XY location of the Nudge pin in `[gcode_macro _CALIBRATE_MOVE_OVER_PROBE]`

6. Save and restart

7. Mount toolhead **T0** and make sure it's nozzle is clean

8. **<mark>!!! MAKE SURE THE NUDGE PROBE IS NOT MOUNTED !!!</mark>** - only applicable to the removable Nudge setup

9. Run `G28` and `QUAD_GANTRY_LEVEL`

10. Mount the Nudge probe

11. Run `CALIBRATE_NPO` - **BUT, DO NOT SAVE**

12. Look at the proposed offset and compare it to the recorded `z_offset` from step 1. Take the different and put it in the `trigger_to_bottom_z:` in `[tools_calibrate]`. Be mindful of the direction, remember: **Decrease -> higher nozzle**.

13. Restart and repeat steps 8-10 until the proposed z-offset to be +-0.01mm of the recorded `z_offset` in step 1

Your Nudge probe is ready.

### 4.4. Other toolheads

1. Mount toolhead T0 and make sure it's nozzle is clean

2. <mark>**!!! MAKE SURE THE NUDGE PROBE IS NOT MOUNTED !!!**</mark> - only applicable to the removable Nudge setup

3. Run `G28` and `QUAD_GANTRY_LEVEL`

4. Make sure all nozzles are clean

5. Go to each toolhead config, comment out:
   
   - gcode_x_offset
   
   - gcode_y_offset
   
   - gcode_z_offset
   
   - z_offset

6. Save it, <mark>**but don't restart**</mark>

7. Mount the Nudge probe

8. Run `CALIBRATE_OFFSETS` - the process is automatic, and you can specify a specific tool-head

9. Run `SAVE_CONFIG`

## 5.

TBD
