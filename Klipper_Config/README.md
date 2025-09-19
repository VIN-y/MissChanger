# Table of Contents

1. [Introduction](#1-introduction) 

2. [Versions](#2-versions) 

3. [Software](#2-software) 
   
   3.1. [Installation](#31-installation) 
   
   3.2. [Configuration](#32-configuration) 

4. [Calibration](#4-calibration) 
   
   4.0. [Tap&Change burn-in](#40-tapchange-burn-in) 
   
   4.1. [Park position calibration](#41-park-position-calibration) 
   
   4.2. [Input Shaper (optional)](#42-input-shaper-optional) 
   
   4.3. [Calibrate offsets](#43-calibrate-offsets) 

5. [Test and Troubleshoot](#5-test-and-troubleshoot) 
   
   5.1. [Offsets](#51-offsets) 

6. [Tool-change Tuning](#6-tool-change-tuning) 

7. [Slicer Profile](#7-slicer-profile) 

---

# 1. Introduction

Although setting up the config for MissChanger will change some aspects of your printer, it is still recommended that you start the project with a functional printer with Voron-Tap, then add MissChanger and one tool-head at a time.

This document will not guide you through the set up of CAN bus or the physical mounting of the related hardware (i.e. U2C and tool-head board), please refer to the manufacture’s manual for that.

---

# 2. Versions

There are 3 versions for the software, the sample user configs for which are placed in the relevant folder: [1_Main](./1_Main) / [2_Beta](./2_Beta) / [3_Alpha](./3_Alpha).

**Note:** User configs are files that will needed to be set-up on the user's side.

---

# 3. Software

This section aims to provide a guide through the installation process of the software and setup the config.

## 3.1. Installation

To install the [klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger) plugin, run the following installation script using the following command over SSH. This script will download this GitHub repository to your RaspberryPi home directory, and symlink the files in the Klipper extra folder.

Before installation. For those who are switching branches. You will need to run the uninstall script in below, in **section 3.1.2**.

---

### 3.1.1. Install

1. For normal use:

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/main/scripts/install.sh | bash
```

2. For Beta test:

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/beta/scripts/install.sh | bash
```

3. For Alpha test:

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/alpha/scripts/install.sh | bash
```

*Note 1: You will need to reboot the printer whenever there is an update for the add-on.*

---

### 3.1.2. Uninstall

Some manual changes will still need to be done in the user space (i.e. the section that is accessible via the web interface). These settings / files / folders tend to contain information and calibration values specific to your printer. Thus, it is advised that you create a backup before deletion. This includes:

* The `config` folder

* The `misschanger_settings.cfg` file

* Relevant tool-head config file

* Relevant settings in `printer.cfg`

* `moonraker.conf` registry

To fully uninstall the back-end, run one of the following commands over SSH. Make sure to select the link that is relevant to the version that is currently installed on the printer.

1. For normal use:

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/main/scripts/uninstall.sh | bash
```

2. For Beta test:

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/beta/scripts/uninstall.sh | bash
```

3. For Alpha test:

```
wget -O - https://raw.githubusercontent.com/VIN-y/klipper-toolchanger/alpha/scripts/uninstall.sh | bash
```

---

## 3.2. Configuration

The **sample configuration files** can be found in the relevant folder above.

The following, starting from **Step 1**, are the recommended steps to get the software up-and-running using the default set of macros. Following the guide will make it easier for you to get support.

The default macros will be managed and updated together with the software stack.

---

### Step -1: Customisation

If you are comfortable with it, you are welcome to by-pass the default macros and play with the configs files to your liking. If you find any errors or room for improvement, feel free to reach out to me to get it fixed, via a GitHub pull request or otherwise.

For building your custom macro:

1. Check where the relevant macro is saved, using the [code stack breakdown](./code_stack_breakdown.pdf) 

2. Download the relevant macro config file, in the [klipper-toolchanger](https://github.com/VIN-y/klipper-toolchanger/tree/main/macros) repository (or straight from your web interface)

3. Import the config file back using the web interface

4. `include` it back into`printer.cfg`

5. Comment-out (or delete) the reference to the file that the new config replaces, in `printer.cfg`

6. Save and Restart

7. Start editing the file to your liking

---

### Step 0: Pre-configuration

These are settings that are in the default Klipper that should to be enabled:

1. [exclude_object] - [Exclude Object](https://youtu.be/QTwRZ_M159Q?si=oV987KDLM-wXsYoE), video guide by [ModBot](https://www.youtube.com/@ModBotArmy) 
2. [skew_correction] - calibrate with [Vector3D CaliLantern](https://vector3d.shop/products/calilantern-calibration) 
3. [input_shaper] - see section **4.2.** 
4. [bed_mesh] - This should have already been configured. However, make sure that there is a bed mesh profile named "default", for the PRINT_START to fallback to if needed.

---

### Step 1: Back-up your running system

Through your web interface:

1. Select all of your config files

2. Download it to your computer

---

### Step 2: Set up printer.cfg

You can check the sample `printer.cfg` in the relevant folder above as reference, and import `macro-general.cfg` and `macro-testing.cfg` if wanted (**Note:** exclude the relevant *.cfg if not used).

Each variable has been given a short description on what they do. Some variables can be used to disable functionalities, such that you will not need to comb through the configs to find and edit them out. The key sections are:

1. Add the **Section Variables** section.
   
   These sections need to be placed just before the **SAVE_CONFIG** section, as shown in the sample `printer.cfg`. Everything below the `Section Variable marker` will be swapped in and out upon the `CONFIG_TOGGLE` macro. If a function setting already exists somewhere else in printer.cfg, the old function will need to be transferred to the new location and updated.
   
   The critical settings that need to be changed are as follows:
  - `[gcode_macro _home]` needs to adjusted with the appropriate `xh` and `yh` which represents the centre of the new build area (i.e. 150, 85 for a 300x300 bed).
  
    - `variable_dock` is the indicator of which config is being used.
    
      - Before installing the dock and other tool-heads. Set this to `False`. Then, set up T0 (the reference tool-head), see **Step 3**. The `[quad_gantry_level]` or `z_tilt` (for Trident), and `[bed_mesh]` stay the same the same as stock.
    
      - After T0 is operational, run `CONFIG TOGGLE`
    
      - Then, install the physical dock. The `[quad_gantry_level]` or `z_tilt` (for Trident), and `[bed_mesh]` **MUST** be updated, as shown in the next 2 bullet points.

  - `[quad_gantry_level]`, or `z_tilt` (for Trident), increase the y position of the front 2 `points:` to `130`, to avoid crashing into the dock.

  - `[bed_mesh]`, make sure that `mesh_min: 30,130` , to avoid crashing into the dock.
2. If it exists in **printer.cfg**, disable `[safe_z_home]` (comment-out or delete)

3. Add relevant variable (see the code snippet below) under **SAVE_CONFIG**. `[tool_probe T0]` and  `[extruder]` are needed no matter what your setup as it is the reference / default tool-head. 

4. For each additional tool-head, you will need an associated set of  `[tool t(x)]`, `[tool_probe T(x)]`, and `[extruder(x)]` sections. **It is RECOMMENDED** to add and test the tool-heads, one at a time.

*NOTE 1: Your printer will be throwing errors at this point, until the software is fully setup.* 

*NOTE 2: The variables will need to be calibrated to match your hardware.*

---

### Step 3: Make the reference tool-head config file

Use the `T0-SB2209-Revo-LDO.cfg` in [Sample_Config](./Sample_Config) folder as reference.

1. Transfer the items relating to the tool-head from your `printer.cfg` to a separate file:
   
   * `[mcu EBBCan0]` - the MCU for your tool-head, named to your liking, i.e. EBBCan0.
   
   * `[adxl345]` - The accelerometer on the tool-head.
   
   * `[resonance_tester]` 
   
   * `[temperature_sensor]`
   
   * `[extruder]`
   
   * `[tmc2209]` - that is associated extruder motor
   
   * `[verify_heater]`
   
   * `[heater_fan]`
   
   * `[neopixel]` - if applicable
   
   * `[tool_probe]` 
     
     <mark>NOTE:</mark> the `activate_gcode:` for the item has been replace with the macro `_TAP_PROBE_ACTIVATE`, with the flag `HEATER=extruder`. This flag <mark>NEEDS TO BE SET TO THE RIGHT "EXTRUDER"</mark>. Failure to do so may result in damage bed surface.

2. Replace `[fan]` with `[fan_generic T0_partfan]`

3. Add the following item and macro:
   
   * `[gcode_macro T0]` - for tool switching
   
   * `[tool T0]` - associate the items above to T0 and provide tool specific variables (which will need to be adjusted later).

---

### Step 4: 

Include tool-head config in the session variables are in `printer.cfg`, as shown in the relevant sample config.

*Note: At this point, the printer should be able to have a firmware restart without (or with minor) errors.* 

---

### Step 5: Calibrate and test T0

1. For safety reasons, remove the dock and any attached tool-head other than T0.

2. Run `FIRMWARE_RESTART` and fix any problem that arise ;)

3. Run `G28` and `QUAD_GANTRY_LEVEL` 

4. Check that:
   
   * The printer homes Y before X
   
   * Quad gantry level front points are at Y = 130
   
   * Extruder PID is correctly set and applied (no error / no bouncing temperature)

5. Re-mount the dock

6. Calibrate extruder `rotation_distance`

7. Calibrate heater PID

8. Calibrate the park position, see **section 4.1.** 

9. Calibrate the `z_offset`, see **section 4.3.** 

10. (optional) Calibrate input shaper, see **section 4.2.** 

11. Save & Restart

---

### Step 6: Make the next tool-head and its config file

1. Follow the hardware assembly guide from the manufacture of the relevant hardware, i.e extruder, tool-head board, etc.

2. Mount the dock and temporarily mount the new tool-head to the dock.
   
   <mark>CAUTION:</mark> **DO NOT** try to make the printer pick up the new tool yet.

3. Use `T1-SB2209-Revo-LDO.cfg` (and others) config file as references. Make, calibrate, and test the config files for the other tool-heads.

4. Include tool-head config in the session variables in `printer.cfg`, as shown in the code block in **step 2.3**.

5. Copy and paste the associated set of `[tool t(x)]`, `[tool_probe T(x)]`, and `[extruder(x)]` to the **SAVE_CONFIG** section, if they are not already there.

6. Save & Restart

---

### Step 7: Calibrate and test tool-head

If this is the first tool-head you made beside the reference tool-head, then you will need to test the max y position of your set up:

1. Run `G28`

2. Manually nudge the tool-head to right behind (but not touching) a docked tool (via the web interface), 

3. Note down the Y position of that location

4. If the y location is greater than 120 mm, then you will need to update the `params_safe_y` value in `[toolchanger]` in `misschanger_settings.cfg`

5. Save & Restart

Otherwise:

1. Calibrate extruder `rotation_distance`

2- Calibrate heater PID

3. Save & Restart

4. Run `G28` and `QUAD_GANTRY_LEVEL`

5. Calibrate the park position, see **section 4.1.** 

6. Calibrate the `z_offset`, see **section 4.3.** 

7. (optional) Calibrate input shaper, see **section 4.2.** 

---

### Step 8: Other macros

* `macro-general.cfg`

* `macro-test.cfg`

These configs tend to be points of customisation for many. Therefore, the included files are intended to be inspirations for your own macros. They contain commands and functionalities that may not be needed or are not relevant to your printer.

---

### Step 9: Single tool-head config

1. Run `SAVE_CONFIG_MODE` to save the current setup.

2. Check if the save was made correctly, in the **config** folder.

3. In the Section Variable section in **printer.cfg**:
   
   1. Comment out all included tool-head configs other than T0
   
   2. Under `[gcode_macro _home]`:
      
      1. Set an appropriate `xh` and `yh` which represent the centre of the new build area (i.e. 150, 85 for a 300x300 build plate).
      
      2. Set `variable_dock` to `False`
   
   3. Revert `[bed_mesh]` and `[quad_gantry_level]` to the original values of the stock machine (without the tool-changer bits)
   
   4. Delete all the `[tool T(x)]`, `[tool_probe T(x)]`, and `[extruder(x)]` sections EXCEPT the `[tool_probe T0]` and `[extruder]`sections. in the `SAVE_CONFIG` section. 

4. Save, but no restart is needed.

5. Run `SAVE_CONFIG_MODE` and check if the file is correctly saved.

Now, the `CONFIG_TOGGLE` macro will allow you to toggle between these 2 configs.

---

# 4. Calibration

This section assumes that the new tool-head has been assembled, wired up, and has been recognised by Klipper.

## 4.0. Tap&Change burn-in

The following steps are for the burn-in of the Tap&Change mechanism. This is so that it can sort out any misalignment due to print tolerance and assembly. Through testing, it has been shown that this only need to be done once for every Tap&Change rail-side (there is only one in the system).

1. Setup to the variable for `SHAPER_CALIBRATE`, to your liking

2. Manually mount T0

3. Run `G28`

4. Bring the tool-head to 10 mm above the centre of the bed

5. Turn on the bed heater

6. Turn on the chamber fan (if exist)

7. Wait until the tool-head (i.e. "Extruder") temperature reached ~40°C (or ~10 mins if you don't have a chamber temperature sensor)

8. Run `SHAPER_CALIBRATE` - Note: This is only meant to shake the tool-head around in its mount. Do not save the input shaper result.

9. Repeat step 4 - 8 until the tool-head can fall into the mount on it's own weight when manually removed and replaced.

---

## 4.1. Park position calibration

1. Run `G28` and `QUAD_GANTRY_LEVEL`  - if not already done

2. Run `G1 Z150 F9000` 

3. Run `STOP_TOOL_PROBE_CRASH_DETECTION` 

4. Loosen the bolts on the bottom of the dock of the relevant dock (so it will spin around the bar)

5. On the web interface, in `Setting > Control` add `0.1` and remove `100` in the `Move distance increments X & Y axes (in mm)` 
   
   *Note: This is temporary for this step only, and can be reverted later.*

6. Set the dock toolhead cradle at the 'empty' position in the dock assembly, i.e. at the bottom of the ramp.
   ![](./images/20240730_194812.jpg)

7. Use the web interface, nudge the tool-head into the correct x-position. This can be tested by nudging the y-position in and out to see which side of the tool-head touches the dock first. Then adjust the x-position such that both sides of the dock touch the tool-head at the same time.
   ![](./images/20240730_195442.jpg)

8. Copy and paste the current x-coordinate into the `params_park_x:` of the config file of the current tool-head

9. Save it, **BUT DON'T RESTART**

10. Nudge the y-position until the toolhead is fully seated in the dock toolhead cradle.
    
    1. Make sure that the whole dock-bar isn't being lifted up in the extrusion grooves.
    
    2. Make sure the toolhead is fully seated in the dock toolhead cradle before it starts being lifted up the ramp. Check it by nudging it back and forth 10mm.
    
    3. If step 10.1 and 10.2 are not met, adjust the Bar End spacers to fit. TIP: the distance for the spacers should be the same for both sides.

11. Tighten the bolts on the bottom of the dock (locking it in place)

12. Nudge the y-position until the dock is at the peak of the ramp. The dock is tough enough to handle a bit of pressure from the gantry so you can keep nudging the y-position until the dock flexes a bit, then back it off.

13. Run:
    
    ```
    G91
    G1 Y+80 F6000
    G90
    ```

14. Copy and paste the current y-coordinate into the `params_park_y:` of the config file of the current tool-head

15. Save it

16. Run `FIRMWARE_RESTART`

17. Run the `TEST_DOCKING` macro

---

## 4.2. Input Shaper (optional)

*Note: To avoid Klipper from throwing errors, the parameters for input shaper are pre-populated in* `toolchanger.cfg` *and in each tool-head config file. Nevertheless, it is best to calibrate it for each available tool-head.*

1. Enable (un-comment) the `[adxl345]` and `[resonance_tester]` in the config of the tool-head that is to be calibrated

2. Disable (comment out) the `[adxl345]` and `[resonance_tester]` in the config of the other tool-heads

3. Save & Restart

4. Mount the tool-head that is to be calibrated

5. Run `G28` and `QUAD_GANTRY_LEVEL`

6. Run `SHAPER_CALIBRATE` - **BUT, DO NOT SAVE**

7. From the console output, save the following parameters into the tool-head config (at the bottom of the `[tool Tx]` section)
   
   ```
   params_input_shaper_type_x: ...
   params_input_shaper_freq_x: ...
   params_input_shaper_damping_ratio_x: ...
   params_input_shaper_type_y: ...
   params_input_shaper_freq_y: ...
   params_input_shaper_damping_ratio_y: ...
   ```

8. Save & Restart

---

## 4.3. Calibrate offsets

### 4.3.1. Calibrate reference z-offset

1. Mount tool-head T0

2. Go to T0 config file. Make sure:
   
   - `z_offset` under `[tool_probe T0]` is commented out
   
   - Every other offsets to `0`; i.e. `x_offset` and `y_offset`
   
   - Save it, **BUT DON'T RESTART** 

3. Run `G28` and `QUAD_GANTRY_LEVEL` 

4. Run `PROBE_CALIBRATE` on the console and go through the process with <u>the normal paper test</u>.

5. <u></u>Run `SAVE_CONFIG` - which will restart your printer

6. Go to `printer.cfg`

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

8. Delete:
   
   ```
   #*# [tool_probe_endstop]
   #*# z_offset = {value}
   ```
   
   *Note: It will not be used regardless of whether it is there or not.*

9. Save & Restart

*Note: It is key that you get the z_offset correct for the T0, as it will be used to extrapolate other offsets later on. Therefore, it is worth diverging from these instruction if you have a preferred way to set your the z-offset.*

---

### 4.3.2. Calibration probe setup

At this stage, you should have:

- The calibration probe assembled, tested, and installed

- T0 z-offset calibrated

This section will guide you through the calibration of the machine specific variable `trigger_to_bottom_z`, which will allow you to automate the z-offset of the tool-heads that are not T0.

*Note: This should be your go to variable to adjust whenever you ran into z-offset issue.*

### Setup:

1. Open `misschanger_settings.cfg`

2. Setup `[tools_calibrate]` with the correct `pin`

3. Save and restart

4. Open `printer.cfg` 

5. Set the approximate X and Y location of the Nudge pin in `[gcode_macro _static_variable]` 

6. Save and restart

### Steps:

1. Go to `printer.cfg` and record the `z-offset` for `[tool_probe T0]`, which should be at (or near) the bottom of the file

2. Mount tool-head **T0** and make sure it's nozzle is clean

3. Run `G28` and `QUAD_GANTRY_LEVEL`

4. Mount the calibration probe

5. Run `CALIBRATE_TRIGGER_BOTTOM`. If it is a fresh calibration probe, it is worth redoing this step until the suggested value is consistent at +/- 0.005mm. 

6. Copy the proposed offset on the console to the `trigger_to_bottom_z:` in `[tools_calibrate]`.

7. Save and restart

8. Run `CALIBRATE_OFFSETS TOOL=0` - But, **DON'T RESTART**

9. Check if proposed z-offset to be +-0.01mm of the recorded `z_offset` in **step 1**.

10. Run `CALIBRATE_OFFSETS TOOL=0` 2-3 more times to make sure the measured value is consistent.

Your calibration probe is ready.

*Note: It is worth mentioning that there are other ways to calibrate your offsets, such as the visual based solution from [Ember](https://www.emberprototypes.com/products/cxc) or the calibration print that you can get from [Printables](https://www.printables.com/model/201707-x-y-and-z-calibration-tool-for-idex-dual-extruder-). Each have their pros and cons in terms of accuracy and cost. However,  a physical contact probe is currently the only way to automatically calibrate all relevant offsets.*

---

### 4.3.3. Other tool-head(s)

### Steps:

1. <mark>**!!! MAKE SURE THE CALIBRATION PROBE IS NOT MOUNTED !!!**</mark>

2. Make sure all tool-heads are cold (near room temperature)

3. Make sure all nozzles are clean

4. Make sure all tool-head config file (except T0) has the following variables disabled (commented out):
   
   - `gcode_x_offset` 
   
   - `gcode_y_offset` 
   
   - `gcode_z_offset` 
   
   - `z_offset` 

5. Save it, **BUT DON'T RESTART**

6. Mount tool-head T0

7. Run `G28` and `QUAD_GANTRY_LEVEL`

8. Mount the Nudge probe

9. Run `CALIBRATE_OFFSETS` - the process is automatic, but you can specify a specific tool-head

10. Run `SAVE_CONFIG`

If you has the set `variable_calibration_abs_z_seperately` to `1` in `[gcode_macro _static_variable]` in **printer.cfg** (for the Nudge probe).

1. Mount tool-head T0

2. Run `G28` and `QUAD_GANTRY_LEVEL` 

3. Run `CALIBRATE_ABSOLUTE_Z` 

4. Run `SAVE_CONFIG`

---

### 4.3.4. Clean dock

#### Steps:

1. Install the clean dock in your preferred slot on the gantry

2. Approximate the x position of the centre of the dock (where the brush is)

3. Add the aforementioned x position into `printer.cfg`. In `variable_clean_dock_x` under `[gcode_macro _static_variable]`.

#### Optional steps, for auto mid-print clean:

1. Set-up the `variable_clean_threshold` under `[gcode_macro _static_variable]`. "150" means that the print needs to take up ~50% of the bed before triggering a mid-clean print.

2. List the materials that need to have their associated tool-head cleaned in `misschanger_settings.cfg`. For example:
   
   ```
   params_need_clean_materials: ['PETG', 'FLEX']
   ```

---

# 5. Test and Troubleshoot

## 5.1. Offsets

It is important to test if the `z_offset` and `gcode_z_offset` are set-up and applied correctly. A misconfigured `z_offset`/`gcode_z_offset` can cause the nozzle to be dragged on the bed, risking damage. Please follow the steps below to test the config, before you start printing.

### Part 1: Check the z_offset of T0

1. Make sure the printer is cold (near ambient temperature).

2. Run `G28` and `QUAD_GANTRY_LEVEL` with T0

3. Run `G1 X175 Y235 Z10 F9000` (centre of print area)

4. Run `G1 Z0 F9000` 

5. Test the z-offset with the paper test. - If it is good then you set up the `trigger_to_bottom_z` value correctly.

6. If it does not work:
   
   1. baby step the z position to the correct position
   
   2. adjust `trigger_to_bottom_z` accordingly using the baby stepped amount.
   
   3. then, rerun `CALIBRATE_OFFSETS TOOL=0`.
   
   4. retry step 1 to 3 until the paper test succeeds.

### Part 2: Check the gcode_z_offset of the other tools

1. Go through step 1 and 2 of **Part 1**, above.

2. Switch to next tool, via the web UI or Klipper Screen.

3. Go through step 3 to 6 of **Part 1**, above.

4. If it does not work. rerun `CALIBRATE_OFFSETS TOOL=[x]` for that tool-head (or all of them).

### Part 3: gcode_x_offset and gcode_y_offset

To validate `gcode_x_offset` and `gcode_y_offset`, you just need to print something and see if they are set-up and applied correctly. Alternatively, you can also buy and use [Ember Camera Assisted XY](https://www.emberprototypes.com/products/cxc) to validate them.

---

# 6. Tool-change Tuning

The speed and path of the default tool-change routine in `misschanger_settings.cfg` is tuned for reliability. It is slower and has more steps than needed.

For a smooth running MissChanger. The `params_path_speed` can be increased and some of the "Wiggle wiggle", in the path, can be disable.

---

# 7. Slicer Profile

## 7.1. Custom Start G-code

Copy and paste the following code into your slicer.

*Note: It needs to stay as a single line of gcode.*

```
PRINT_START BED_TEMP=[first_layer_bed_temperature] FIRST_LAYER_PRINT_SIZE=[first_layer_print_size] TOOL=[initial_tool] TOOL_TEMP={first_layer_temperature[initial_tool]} {if is_extruder_used[0]}T0_TEMP={first_layer_temperature[0]} T0_Fil={filament_type[0]}{endif} {if is_extruder_used[1]}T1_TEMP={first_layer_temperature[1]} T1_Fil={filament_type[1]}{endif} {if is_extruder_used[2]}T2_TEMP={first_layer_temperature[2]} T2_Fil={filament_type[2]}{endif} {if is_extruder_used[3]}T3_TEMP={first_layer_temperature[3]} T3_Fil={filament_type[3]}{endif} {if is_extruder_used[4]}T4_TEMP={first_layer_temperature[4]} T3_Fil={filament_type[4]}{endif} {if is_extruder_used[5]}T5_TEMP={first_layer_temperature[5]} T5_Fil={filament_type[5]}{endif}
```

![](./images/start_gcode.png)

Also. Disable the following option:

![](./images/Temp_emits.png)

## 7.2. Slicer Bed Shape

The printer bed shape need to be set as shown below to avoid collisions with the dock during printing.

Depending on the umbilical mounting solution on the tool-head side, the lost y can be between 120mm to 140mm.

*Note: It can be seen here that the max print height was set to 235mm. This is recommended for new machines to avoid umbilical tangling. However, if you are willing to do some tuning to the tension of the umbilical or add a top hat to the system, the max print height can be increased to 250mm or 300mm.*

### For printers with 350mm beds:

![](./images/Slicer_bed_shape_350.png)

### For printers with 300mm beds:

![](./images/Slicer_bed_shape_300.png)

## 7.3. Multiple Extruders

Enable **Ooze Prevention**.

This setting will allow you to reduce the docked nozzle temperature, to prevent over cooking the filament. Furthermore, it will effectively act as reheat command for when the system is recovering from a failed tool-change.

Even if you don't want it to drop the temperature, still enable it, then set the `Temperature variation` to -25°C. This setting will emit a nozzle heating command after tool-change.

![](./images/Ooze_prevention.png)

## 7.4. Speed profile

Although, you might want to tune your own speed profile for the best performance. Here is a slow and reliable profile to get you started with.

![](./images/Slicer_Speed.png)
