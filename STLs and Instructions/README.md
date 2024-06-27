# INSTRUCTION

## 1. Hardware

Assembly and wiring instructions for each version are in the respective folder above.

## 2. Software

TBD

## 3. Calibration

### 3.1. Calibrate Reference Toolhead

1. Mount toolhead T0

2. Go to T0  config file
   
   - Comment out the `z_offset` under `[tool_probe T0]` 
   
   - Set every other offsets to 0
   
   - Save it, **BUT DON'T RESTART**

3. Run `PROBE_CALIBRATE` on the console and go through the process with the normal paper test

4. Run `SAVE_CONFIG` - which will restart your printer

*Note: it is key that you get the z_offset correct for the T0, as it will be used to extrapolate other offsets later on. Therefore, it is worth diverge from the instruction, if you have a preferred way to set your the z-offset.*

### 3.2. Nudge Probe Calibration

At this stage, you should have:

- The Nudge probe assembled, tested, and installed

- T0 z_offset calibrated

This section will guide you through the calibration of the `trigger_to_bottom_z` for the probe, which will allow you to automate the z_offsets of the toolheads that are not T0. This should be your go to variable to adjust whenever you ran into z-offset issue.

#### Steps:

1. Go to **printer.cfg** and record the `z-offset` for `#*# [tool_probe T0]`, which should be at (or near) the bottom of the file

2. Download the [calibrate-offsets.cfg](https://github.com/VIN-y/MissChanger/blob/main/Software/calibrate-offsets.cfg), add it into your config folder, and include it into the **printer.cfg** file:
   
   ```
   [include calibrate-offsets.cfg]
   ```

3. Open `calibrate-offsets.cfg`

4. Set it to the correct pin in `[tools_calibrate]`

5. Set the approximate XY location of the Nudge pin in `[gcode_macro _CALIBRATE_MOVE_OVER_PROBE]`

6. Save and restart

7. Mount toolhead **T0** and make sure it's nozzle is clean

8. **<mark>!!! MAKE SURE THE NUDGE PROBE IS NOT MOUNTED !!!</mark>** 

9. Run `G28` and `QUAD_GANTRY_LEVEL`

10. Mount the Nudge probe

11. Run `CALIBRATE_NPO` - **BUT, DO NOT SAVE**

12. Look at the proposed offset and compare it to the recorded `z_offset` from step 1. Take the different and put it in the `trigger_to_bottom_z:` in `[tools_calibrate]`. Be mindful of the direction, remember: **Decrease -> higher nozzle**.

13. Restart and repeat steps 8-10 until the proposed z-offset to be +-0.01mm of the recorded `z_offset` in step 1

Your Nudge probe is ready. 

*Note: It is worth mentions that there are other ways to calibrate your offsets beside the Nudge, such as the visual based solution from [Ember](https://www.emberprototypes.com/products/cxc) or the calibration print that you can get from [Printables](https://www.printables.com/model/201707-x-y-and-z-calibration-tool-for-idex-dual-extruder-). Each with their pros and cons, in term of accuracy and cost. However, Nudge is currently the only way to calibrate all relevant offsets.*

### 3.3. Other toolheads

#### Steps:

1. Mount toolhead T0 and make sure it's nozzle is clean

2. <mark>**!!! MAKE SURE THE NUDGE PROBE IS NOT MOUNTED !!!**</mark> 

3. Run `G28` and `QUAD_GANTRY_LEVEL`

4. Make sure all nozzles are clean

5. Go to each toolhead config, comment out:
   
   - gcode_x_offset
   
   - gcode_y_offset
   
   - gcode_z_offset
   
   - z_offset

6. Save it, **BUT DON'T RESTART**

7. Mount the Nudge probe

8. Run `CALIBRATE_OFFSETS` - the process is automatic, and you can specify a specific tool-head

9. Run `SAVE_CONFIG`
