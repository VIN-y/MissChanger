# INSTRUCTION

## 1. Glossary

| Terminology  | Definitions                                                                                       |
|:------------ | ------------------------------------------------------------------------------------------------- |
| probe offset | The distance between the trigger point of the XYZ probes / endstops and their expected position.  |
| tool offset  | The nozzle position and the relative to the position of the nozzle of the reference toolhead, T0. |
| T0           | Reference toolhead                                                                                |
| G28          | this command to home all (XYZ).                                                                   |



## 2. Assembly

TBD



## 3. Calibration

### 3.1. The basis

1. There is a default command for z-offset calibration, `PROBE_CALIBRATE`. It is part of default Klipper and is needed for the initial calibration.

2. There are 2 type of offsets for each tool-head:
   
   - **x_offset** / **y_offset** / **z_offset** - which are the default offsets that most people are used to, which will be referred to as probe offset moving forward. They are sort of like hard-coded values (they are not); in that, they are pretty hard to work with downstream, i.e. in the configs.
   
   - **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** - these are like the an on-the-fly adjustment to for the gcode. This the stuff you adjust when you do baby-stepping mid-print.

3. For the purpose of the tool-changer:
   
   - **x_offset** and **y_offset** will not be used.
   
   - **z_offset** is to be calibrates for all toolheads.
   
   - **gcode_x_offset** / **gcode_y_offset** / **gcode_z_offset** are used to account for the XYZ different between the nozzles, based on a reference nozzle.



### 3.2. Calibrate Reference Toolhead

1. Mount toolhead T0.

2. Go to T0  config file.
   
   - Comment out the `z_offset` under `[tool_probe T0]` 
   
   - Set every other offsets to 0.
   
   - Save it, <mark>**but don't restart**</mark>.

3. Run `PROBE_CALIBRATE` on the console and go through the process with the normal paper test.

4. Run `SAVE_CONFIG` - which will restart your printer.

*Note: it is key that you get the z_offset correct for the T0, as it will be used to extrapolate other offsets later on. Therefore, it is worth diverge from the instruction, if you have a preferred way to set your the z-offset.*



### 3.3. Nudge Probe Calibration

*Note: It is worth mentions that there are other ways to calibrate your offsets beside the Nudge, such as the visual based solution from [Ember](https://www.emberprototypes.com/products/cxc) or the calibration print that you can get from [Printables](https://www.printables.com/model/201707-x-y-and-z-calibration-tool-for-idex-dual-extruder-). Each with their pros and cons, in term of accuracy and cost. However, Nudge is currently the only way to calibrate all relevant offsets.*



At this stage, you should have:

- The Nudge probe assembled, tested, and installed.

- T0 z_offset calibrated.



This section will guide you through the calibration of the `trigger_to_bottom_z` for the probe, which will allow you to automate the z_offsets of the toolheads that are not T0.

1. Go to **printer.cfg** and record the `z-offset` for `#*# [tool_probe T0]`, which should be at (or near) the bottom of the file.
2. download the [calibrate-offsets.cfg](https://github.com/viesturz/tapchanger/blob/main/Klipper/config-example/calibrate-offsets.cfg) from Viesturz.
3. Set it to the correct pin in `[tools_calibrate]`.
4. Set the approximate XY location of the Nudge pin in `[gcode_macro _CALIBRATE_MOVE_OVER_PROBE]`.
5. Remove all `_CLEAN_NOZZLE TEMP=220` from the file, if you don't have that macro.
6. Save and restart.
7. Mount toolhead T0 and make sure it's nozzle is clean.
8. Run `G28` and `QUAD_GANTRY_LEVEL`.
9. Run `CALIBRATE_NOZZLE_PROBE_OFFSET` - **DO NOT SAVE**.
10. Look at the proposed offset and compare it to the recorded `z_offset` from step 1. Take the different and put it in the `trigger_to_bottom_z:` in `[tools_calibrate]`. Be mindful of the direction, remember: **Decrease -> higher nozzle**.
11. Restart and repeat steps 8-10 until the proposed z-offset to be +-0.01mm of the recorded `z_offset` in step 1. 



Your Nudge probe is ready.



### 3.4. Other toolheads

1. Mount toolhead T0 and make sure it's nozzle is clean.

2. Run `G28` and `QUAD_GANTRY_LEVEL`.

3. Make sure all nozzles are clean.

4. Go to each toolhead config, comment out:
   
   - gcode_x_offset
   
   - gcode_y_offset
   
   - gcode_z_offset
   
   - z_offset

5. Save it, <mark>**but don't restart**</mark>.

6. Run `CALIBRATE_ALL_OFFSETS`. - the process is automatic.

7. Run `SAVE_CONFIG`



## 4. Software

TBD
