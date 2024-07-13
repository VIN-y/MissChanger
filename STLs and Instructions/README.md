# INSTRUCTION

## 1. Introduction

Although the setting up the config for MissChanger will change aspect of your printer, it is still recommended that you start the project with a functional printer and add one toolhead at a time.

This document will not guide you through the set up of CAN bus, please refer to the manufacture manual for that.

## 2. Hardware

Assembly and wiring instructions for each version are in the respective folder above.

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

The configuration files can be found in the [Software](https://github.com/VIN-y/MissChanger/tree/main/Software) folder, where further descriptions of each config file are also placed. The steps are as follow:

#### 3.2.1 Create the session variable section in `printer.cfg`

This section need to be just before the `SAVE_CONFIG` section, as shown in the sample `printer.cfg`. The content of the session variable section is as follow:

```
[config_switch]

####################################################################################
#                          Session Variables
####################################################################################
#;<-----------------session_variable_marker---------------------

# [include macro-tc-nozzle_clean.cfg]   # Uncomment if macro is used
# [include ./T0-SB2209-Revo-LDO.cfg]    # Uncomment and rename when ready
# [include ./T1-SB2209-Revo-LDO.cfg]    # Uncomment and rename when ready

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

#---------------------------------------------------------------
[gcode_macro _WIPE_TOWER_LOCATION]
variable_wx: 0.0
variable_wy: 0.0
gcode:
    RESPOND TYPE=echo MSG="WIPE_TOWER_X = {wx}"
    RESPOND TYPE=echo MSG="WIPE_TOWER_Y = {wy}"
```

*Note: As you may have notice, this section includes `[include]` functions, which is typically placed at the top of the file, and the `[quad_gantry_level]`  function, which you may already have in your `printer.cfg`.*

* If you already have `[quad_gantry_level]` before-hand, then you can cut and paste it in place of the one in the session variable section. <u>However, you will need to increase the y position of the  front 2</u> `points:` <u>to 130, as shown above, to avoid crashing into the dock.</u>

#### 3.2.2. Add the necessary config

The following files are contains the key settings and macros that are needed for the printer:

* `calibrate-offsets.cfg`

* `homing.cfg` 

* `toolchanger.cfg`
1. Add them to your folder and include them into your `printer.cfg`, at the top:
   
   ```
   [include homing.cfg]
   [include calibrate-offsets.cfg]
   [include toolchanger.cfg]
   ```

2. Also in `printer.cfg`, disable `[safe_z_home]` (commented-out or deleted).

*Note: Further configuration in the `calibrate-offsets.cfg`  will be needed to calibrate the calibration probe, see section 3.*

#### 3.2.3. Make the reference toolhead config file

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

3. Include the config into the session variable, as shown in the code block in section 2.1.

*Note: At this point, the printer should be able to have a firmware restart without (or with minor) errors.*

#### 3.2.4. Calibrate and Test T0

1. Remove the dock and any attached toolhead other than T0 (for safety reason).

2. Run `FIRMWARE_RESTART` , and fix any problem that arise.

3. Run `G28` and `QUAD_GANTRY_LEVEL`.

4. Check:
   
   * The printer home Y before X
   
   * Quad gantry level front points are at Y130
   
   * Extruder PID is correctly set and applied (no error, no bouncing temperature)

5. Calibrate dock position, see section 3.1.

#### 3.2.5. Make the other toolhead config files

Use `T1-SB2209-Revo-LDO.cfg` and the reference toolhead config file as references. Make, calibrate and test the config files for the other toolheads.

*Note: It is advised this is done one toolhead at a time.*

#### 3.2.6. Other macros

The `macro-*.cfg` files contains the useful macros, such as:

* `PRINT_START`

* `PRINT_END`

* etc.

However, these configs tends to be points of customisation for the user. Therefore, included files are intended to be inspirations for your own macros. The contains commands and functionalities that may not be presented on the user's printer, i.e. lighting and skew profile.

## 4. Calibration

### 4.1. Calibrate Dock Position

TBD

### 4.2. Calibrate Offsets

#### 4.2.1. Calibrate Reference Toolhead

1. Mount toolhead T0

2. Go to T0  config file
   
   - Comment out the `z_offset` under `[tool_probe T0]` 
   
   - Set every other offsets to 0
   
   - Save it, **BUT DON'T RESTART**

3. Run `G28` and `QUAD_GANTRY_LEVEL`

4. Run `PROBE_CALIBRATE` on the console and go through the process with the normal paper test

5. Run `SAVE_CONFIG` - which will restart your printer

*Note: it is key that you get the z_offset correct for the T0, as it will be used to extrapolate other offsets later on. Therefore, it is worth diverge from the instruction, if you have a preferred way to set your the z-offset.*

#### 4.2.2. Nudge Probe Calibration

At this stage, you should have:

- The Nudge probe assembled, tested, and installed

- T0 z_offset calibrated

This section will guide you through the calibration of the `trigger_to_bottom_z` for the probe, which will allow you to automate the z_offsets of the toolheads that are not T0. This should be your go to variable to adjust whenever you ran into z-offset issue.

#### Steps and note:

1. Go to **`printer.cfg`** and record the `z-offset` for `#*# [tool_probe T0]`, which should be at (or near) the bottom of the file

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

12. Restart and repeat steps 8-10 until the proposed z-offset to be +-0.01mm of the recorded `z_offset` in step 1

Your Nudge probe is ready. 

*Note: It is worth mentions that there are other ways to calibrate your offsets beside the Nudge, such as the visual based solution from [Ember](https://www.emberprototypes.com/products/cxc) or the calibration print that you can get from [Printables](https://www.printables.com/model/201707-x-y-and-z-calibration-tool-for-idex-dual-extruder-). Each with their pros and cons, in term of accuracy and cost. However, Nudge is currently the only way to calibrate all relevant offsets.*

#### 4.2.3. Other toolhead(s)

#### Steps and note:

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
