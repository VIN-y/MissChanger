# V1.1

## 1. Introduction

Although the setting up the config for MissChanger will change some aspects of your printer, it is still recommended that you start the project with a functional printer with Voron-Tap, then add MissChanger and one toolhead at a time.

This document will not guide you through the set up of CAN bus or the physical mounting of the related hardware (i.e. U2C and toolhead board), please refer to the manufacture’s manual for that.

## 2. Hardware

### 2.1. MissChanger Core

The following manual is for the core components that are needed to get MissChanger up and running.

MissChanger Assembly Manual: [MissChanger_Assembly_Manual_v1.1.pdf](./MissChanger_Assembly_Manual_v1.1.pdf) 

### 2.2. Optional

These following attachments are extras that will expand the capability of tool-changer system. Nevertheless, they were developed by others and does not share the same design language as MissChanger (i.e. difference print parameters).

* Nevermore Stealth Max (with MissChanger channel switcher user mod) - instruction is TBD 
* DC Barrel Panel Mount - for cable management of the Nevermore Stealth Max

## 3. Software

This section aims to provide a guide through the installation process of the software and setup the config.

### 3.1. Installation

To install the [VIN-y/klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger) plugin, run the following installation script using the following command over SSH. This script will download this GitHub repository to your RaspberryPi home directory, and symlink the files in the Klipper extra folder.

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/main/install.sh | bash
```

Then, add the following to your `moonraker.conf` to enable automatic updates:

```
[update_manager klipper-toolchanger]
type: git_repo
channel: dev
path: ~/klipper-toolchanger
origin: https://github.com/VIN-y/klipper-toolchanger.git
managed_services: klipper
primary_branch: main
install_script: install.sh
```

### 3.2. Configuration

The configuration files can be found in the [Klipper_Config](./Klipper_Config) folder, where further descriptions of each config file can also be found. If you are comfortable with it, you are welcome to dig through and play the configs files in whichever order you refer. Nevertheless, the following are the recommended steps to get the software up-and-running.

#### Step 1: Back-up your system

Through your web interface:

1. Select all of your config files

2. Download it to your computer

#### Step 2: Create the 'Session Variables' section in `printer.cfg`

This section need to be placed just before the 'SAVE_CONFIG' section, as shown in the sample `printer.cfg`. The content of the session variable section is as follow:

```
[config_switch]

####################################################################################
#                          Session Variables
####################################################################################
#;<-------------------------------------------------------------
[include toolchanger-nozzle_clean.cfg]
[include macro-print-multi.cfg]
[include ./T0-SB2209-Revo-LDO.cfg]
[include ./T1-SB2209-Revo-LDO.cfg]
[include ./T2-RP2040-Dragon-Step.cfg]
#---------------------------------------------------------------
[bed_mesh]
speed: 200                   # Calibration speed
horizontal_move_z: 10        # Z-axis movement speed
mesh_min: 30,130             # Minimum calibration point coordinates x, y
mesh_max: 320, 320           # Maximum calibration point coordinates x, y  #350mm=320,320
probe_count: 11,11           # Number of sampling points (7X7 is 49 points)
mesh_pps: 2,2                # Number of supplementary sampling points
algorithm: bicubic           # algorithmic model
bicubic_tension: 0.2         # Algorithmic interpolation don't move
#---------------------------------------------------------------
[quad_gantry_level]
#    Gantry Corners for 350mm Build
gantry_corners:
    -60,-10
    410,420
#  Probe points
points:
    50,130
    50,300
    300,300
    300,130

speed: 150                          # Levelling speed
horizontal_move_z: 15               # Z-axis starting height
retries: 10                         # Number of out-of-tolerance retries
retry_tolerance: 0.0075             # Sampling tolerance
max_adjust: 20                      # Maximum adjustment stroke for levelling
#---------------------------------------------------------------
[gcode_macro _home]
variable_xh: 175.0
variable_yh: 235.0
variable_zh: 10.0
variable_dock: True
gcode:
    RESPOND TYPE=echo MSG='Print area centre: {xh}, {yh}, {zh}'
    RESPOND TYPE=echo MSG='Number of TH: {no_of_toolhead}'

#---------------------------------------------------------------
[gcode_macro _WIPE_TOWER_LOCATION]
variable_wx: 0.0
variable_wy: 0.0
gcode:
    RESPOND TYPE=echo MSG="WIPE_TOWER_X = {wx}"
    RESPOND TYPE=echo MSG="WIPE_TOWER_Y = {wy}"
```

In addition

* For the `[quad_gantry_level]`, increase the y position of the  front 2 `points:` to 130, as shown, to avoid crashing into the dock.
* For the `[bed_mesh]`, make sure that `mesh_min: 30,130` , as shown, to avoid crashing into the dock.

*Note: As you may have notice, this section includes `[include]` functions, which is typically placed at the top of the file, and the `[quad_gantry_level]` and `[bed_mesh]` function, which you should already have in your `printer.cfg` and need to be moved down.* 

#### Step 3: Add the necessary config

The following files are contains the key settings and macros that are needed for the printer, and will need to be add to your config folder via the web interface:

* `toolchanger.cfg`

* `toolchanger-calibrate_offsets.cfg`

* `toolchanger-homing.cfg` 

* `toolchanger-nozzle_clean.cfg`
1. Include these ones under the <u>'Session Variables'</u> section in your `printer.cfg`:

```
[include toolchanger.cfg]
[include toolchanger-homing.cfg]
[include toolchanger-calibrate_offsets.cfg]
```

2. Include this ones to <u>the top</u> of your `printer.cfg`:

```
[include toolchanger-nozzle_clean.cfg]
```

3. Also in `printer.cfg`, disable `[safe_z_home]` (commented-out or deleted).

*Note: Further configuration in the `calibrate-offsets.cfg`  will be needed to calibrate the calibration probe, see **section 4**.* 

#### Step 4: Make the reference toolhead config file

Use the `T0-SB2209-Revo-LDO.cfg` as references.

1. Transfer the items relating to the toolhead from your `printer.cfg` to a separate file:
   
   * `[mcu EBBCan0]` - the MCU for your toolhead, named to your liking, i.e. EBBCan0.
   
   * `[adxl345]` - The accelerometer on the toolhead.
   
   * `[resonance_tester]` 
   
   * `[temperature_sensor]`
   
   * `[extruder]`
   
   * `[tmc2209]` - that is associated extruder motor
   
   * `[verify_heater]`
   
   * `[fan]` - This is to be replaced with `[multi_fan]`
   
   * `[heater_fan]`
   
   * `[neopixel]` - if applicable
   
   * `[tool_probe]` - Notice that the `activate_gcode:` for the item has been replace with the macro `_TAP_PROBE_ACTIVATE`, which can be found in `toolchanger.cfg`

2. Add the following item and macro:
   
   * `[gcode_macro T0]` - for tool switching
   
   * `[tool T0]` - associate the items above to T0 and provide tool specific variables (which will need to be adjusted later).

3. Include toolhead config in the session variables are in `printer.cfg`, as shown in the code block in **step 2**.

4. Copy and paste the following section into the `printer.cfg` under the "SAVE_CONFIG" section, if they are not already there.
   
   ```
   #*# [tool_probe T0]
   #*# z_offset = 0.0
   #*#
   #*# [extruder]
   #*# control = pid
   #*# pid_kp = 36.501
   #*# pid_ki = 4.124
   #*# pid_kd = 80.760
   ```

*Note: At this point, the printer should be able to have a firmware restart without (or with minor) errors.* 

#### Step 5: Calibrate and test T0

1. Remove the dock and any attached toolhead other than T0 (for safety reason).

2. Run `FIRMWARE_RESTART` , and fix any problem that arise ;)

3. Run `G28` and `QUAD_GANTRY_LEVEL` 

4. Check that:
   
   * The printer home Y before X
   
   * Quad gantry level front points are at Y130
   
   * Extruder PID is correctly set and applied (no error, no bouncing temperature)

5. Re-mount the dock

6. Calibrate the park position, see **section 4.1.** 

7. Calibrate the `z_offset`, see **section 4.3.** 

8. (optional) Calibrate input shaper, see **section 4.2.** 

#### Step 6: Make the next toolhead and its config file

1. The hardware assembly for all toolheads are the same

2. Mount the dock and temporary mount the new toolhead to the dock - CAUTION: do not try to make the print pick up the tool yet.

3. Use `T1-SB2209-Revo-LDO.cfg` config file as references. Make, calibrate and test the config files for the other toolheads.

4. Replace the `canbus_uuid` for that of your particular toolhead.

5. Include toolhead config in the session variables are in `printer.cfg`, as shown in the code block in **step 2**.

6. Copy and paste the following section into the `printer.cfg` under the "SAVE_CONFIG" section, if they are not already there. Swap out `[tool T1]`, `[tool_probe T1]`, and `[extruder1]` for the relevant toolhead.
   
   ```
   #*# [tool T1]
   #*# gcode_x_offset = 0.0
   #*# gcode_y_offset = 0.0
   #*# gcode_z_offset = 0.0
   #*#
   #*# [tool_probe T1]
   #*# z_offset = 0.0
   #*#
   #*# [extruder1]
   #*# control = pid
   #*# pid_kp = 36.501
   #*# pid_ki = 4.124
   #*# pid_kd = 80.760
   ```

7. Save & Restart

#### Step 7: Calibrate and test the new toolhead

1. Disable the motors and push the toolhead toward the back - To account for the front of the gantry over sagging

2. Run `G28` and `QUAD_GANTRY_LEVEL`

3. Calibrate the park position, see **section 4.1.** 

4. Calibrate the `z_offset`, see **section 4.3.** 

5. (optional) Calibrate input shaper, see **section 4.2.** 

#### Step 8: Other macros

* `macro-general_0.cfg`

* `macro-general_1.cfg`

* `macro-print-multi.cfg`

* `macro-print-single.cfg`

* etc.

These configs tends to be points of customisation for many. Therefore, the included files are intended to be inspirations for your own macros. They contains commands and functionalities that may not be relevant to user's printer, i.e. lighting, skew profile, chamber thermistor.

## 4. Software Calibration

This section assumes that the new tool-head has been assembled, wired up, and has been recognised by Klipper.

### 4.1. Park position calibration

1. Run `G28` and `QUAD_GANTRY_LEVEL`  - if not already done

2. Run `G1 Z150 F9000` 

3. Run `STOP_TOOL_PROBE_CRASH_DETECTION` 

4. On the web interface, in `Setting > Control` add `0.1` and remove `100` in the `Move distance increments X & Y axes (in mm)` - *Note: This is temporary for this step only, and can be changed back afterwards.*

5. Set the dock at the empty position, i.e. at the bottom of the ramp.
   ![](./images/20240730_194812.jpg)

6. Use the web interface, nudge the toolhead into the correct x-position. This can be tested by nudging the y-position in and out to see which side of the toolhead touch the dock first. Then adjusts it, such that both side of the dock would touch the toolhead at the same time.
   ![](./images/20240730_195442.jpg)

7. Copy and paste the current x-coordinate into the `params_park_x:` of the config file of the current toolhead

8. Save it, **BUT DON'T RESTART**

9. Nudge the y-position until the dock is at the peak of the ramp. - The dock is tough enough to handle a bit of pressure from the gantry. So you can keep nudging the y-position until the dock flex a bit, then back it off.

10. Run:
    
    ```
    G91
    G1 Y+80 F6000
    G90
    ```

11. Copy and paste the current y-coordinate into the `params_park_y:` of the config file of the current toolhead

12. Save it

13. Run `FIRMWARE_RESTART`

14. Run the `A_DOCKING_TEST` macro

### 4.2. Input Shaper (optional)

*Note: To avoid Klippers from throwing errors, the parameters for input shaper are pre-populated in* `toolchanger.cfg` *and in each toolhead config files. Nevertheless, it is best to calibrate it for each available toolhead.*

1. Enable (un-comment) the `[adxl345]` and `[resonance_tester]` in the config of the toolhead that is to be calibrated

2. Disable (comment out) the `[adxl345]` and `[resonance_tester]` in the config of the other toolheads

3. Save & Restart

4. Mount the toolhead that is to be calibrated

5. Run `G28` and `QUAD_GANTRY_LEVEL`

6. Run `SHAPER_CALIBRATE` - **BUT, DO NOT SAVE**

7. From the console output, save the following parameters into the toolhead config (at the bottom of the `[tool Tx]` section)
   
   ```
   params_input_shaper_type_x: ...
   params_input_shaper_freq_x: ...
   params_input_shaper_damping_ratio_x: ...
   params_input_shaper_type_y: ...
   params_input_shaper_freq_y: ...
   params_input_shaper_damping_ratio_y: ...
   ```

8. Save & Restart

### 4.3. Calibrate Offsets

#### 4.3.1. Calibrate Reference Toolhead

1. Mount toolhead T0

2. Go to T0 config file
   
   - Comment out the `z_offset` under `[tool_probe T0]`  - if it is not already is
   
   - Set every other offsets to 0
   
   - Save it, **BUT DON'T RESTART**

3. Run `G28` and `QUAD_GANTRY_LEVEL`

4. Run `PROBE_CALIBRATE` on the console and go through the process with the normal paper test

5. Run `SAVE_CONFIG` - which will restart your printer

6. Go back to `printer.cfg`

7. Copy the z-offset value from:
   
   ```
   #*# [tool_probe_endstop]
   #*# z_offset = {value}
   ```
   
   And, paste it to:
   
   ```
   #*# [tool_probe T0]
   #*# z_offset = {value}
   ```

8. Delete (It will not be used by the system regardless of whether it is there or not):
   
   ```
   #*# [tool_probe_endstop]
   #*# z_offset = {value}
   ```

9. Save & Restart

*Note: it is key that you get the z_offset correct for the T0, as it will be used to extrapolate other offsets later on. Therefore, it is worth diverge from the instruction, if you have a preferred way to set your the z-offset.*

#### 4.3.2. Nudge Probe Calibration

At this stage, you should have:

- The Nudge probe assembled, tested, and installed

- T0 z_offset calibrated

This section will guide you through the calibration of the `trigger_to_bottom_z` for the probe, which will allow you to automate the z_offsets of the toolheads that are not T0. This should be your go to variable to adjust whenever you ran into z-offset issue.

#### Steps and note:

1. Go to `printer.cfg` and record the `z-offset` for `#*# [tool_probe T0]`, which should be at (or near) the bottom of the file

2. Open `calibrate-offsets.cfg`

3. Set it to the correct `pin:` in `[tools_calibrate]`

4. Set the approximate XY location of the Nudge pin in `[gcode_macro _CALIBRATE_MOVE_OVER_PROBE]`

5. Save and restart

6. Mount toolhead **T0** and make sure it's nozzle is clean

7. **<mark>!!! MAKE SURE THE NUDGE PROBE IS NOT MOUNTED !!!</mark>** 

8. Run `G28` and `QUAD_GANTRY_LEVEL`

9. Mount the Nudge probe

10. Run `CALIBRATE_NPO` - **BUT, DO NOT SAVE**

11. Look at the proposed offset and compare it to the recorded `z_offset` from step 1. Take the different and put it in the `trigger_to_bottom_z:` in `[tools_calibrate]`. Be mindful of the direction, remember: **Decrease -> higher nozzle**.

12. Restart and repeat steps 8-10 until the proposed z-offset to be +-0.01mm of the recorded `z_offset` in **step 1**

Your Nudge probe is ready. 

*Note: It is worth mentions that there are other ways to calibrate your offsets beside the Nudge, such as the visual based solution from [Ember](https://www.emberprototypes.com/products/cxc) or the calibration print that you can get from [Printables](https://www.printables.com/model/201707-x-y-and-z-calibration-tool-for-idex-dual-extruder-). Each with their pros and cons, in term of accuracy and cost. However, Nudge is currently the only way to automatically calibrate all relevant offsets.*

#### 4.3.3. Other toolhead(s)

#### Steps and note:

1. Mount toolhead T0 and make sure it's nozzle is clean

2. <mark>**!!! MAKE SURE THE NUDGE PROBE IS NOT MOUNTED !!!**</mark> 

3. Heat all toolhead to 150C and wait ~3 mins for them to stabilised - some hotends have large heat block, which may not be fully thermally expanded.

4. Run `G28` and `QUAD_GANTRY_LEVEL`

5. Make sure all nozzles are clean

6. Go to each toolhead config, comment out:
   
   - gcode_x_offset
   
   - gcode_y_offset
   
   - gcode_z_offset
   
   - z_offset

7. Save it, **BUT DON'T RESTART**

8. Mount the Nudge probe

9. Run `CALIBRATE_OFFSETS` - the process is automatic, and you can specify a specific tool-head

10. Run `SAVE_CONFIG`

## 5. Slicer Profile

The printer bed shape need to be set as shown below, to avoid collisions with the dock during printing.

*Note: It can be seen here that the max print height was set to 235mm. This is recommended for new machines to avoid umbilical tangling. However, if you are willing to do some tuning to the tension of the umbilical or add a top hat to the system, the max print height can be increased to 250mm and 300mm, respectively.*

![](./images/Slicer_bed_shape.png)

Although, you might want to tune your own speed profile for the best performance. Here is a slow and reliable profile to get you started with.

![](./images/Slicer_Speed.png)
