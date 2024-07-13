# Background and Description

This section aims to provide the detail descriptions to each file included in this folder, which are need for MissChanger. The files included in this folder are meant to serve as sample configs, to be read, understood, and modified.

## Config files

### calibrate-offsets.cfg

This file contains:

* `[tools_calibrate]` 
  
  * for the configuration for the calibration routine

* `[gcode_macro _CALIBRATE_MOVE_OVER_PROBE]` 
  
  * It is hidden macro
  
  * to set the approximate location of the calibration probe

* `[gcode_macro CALIBRATE_OFFSETS]`
  
  * for running the full tool offsets calibration routine for all offsets for either all tools or just the selected tool

* `[gcode_macro CALIBRATE_NPO]`
  
  * for calibrating the z-offset of the current tool

### homing.cfg

This file contains:

* `[force_move]`
  
  * allow the printer to move without being homed.

* `[homing_override]`
  
  * custom homing routine, with nozzle and umbilical clearance moves and Y is homed before X.
  * It requires the session variable `_home` in `printer.cfg`

### toolchanger.cfg

This file contains:

* `[tool_probe_endstop]`
  
  * routine for tool crash

* `[toolchanger]`
  
  * configuration for tool change actions
  
  * It requires the session variable `_WIPE_TOWER_LOCATION` in `printer.cfg`
  
  * Input shaper should be ran for each toolhead and manually save to the respective toolhead config file. Alternatively, if all your toolhead are the same and/or the machine is intended to be ran at low speed, you can run the input shaper once, for T0, and save it to the `# Default shaper params` in `toolchanger.cfg`.

* `[gcode_macro M104]`
  
  * overwrite the existing `M104` command to account for multi-toolhead set up.

* `[gcode_macro M109]`
  
  * overwrite the existing `M109` command to account for multi-toolhead set up.

* `[gcode_macro BED_MESH_CALIBRATE]`
  
  * overwrite the existing `BED_MESH_CALIBRATE` command to account for multi-toolhead set up.
  
  * Adaptive bed meshing is on by default

* `[gcode_macro QUAD_GANTRY_LEVEL]`
  
  * overwrite the existing `QUAD_GANTRY_LEVEL` command to account for multi-toolhead set up and the calibration probe.
  
  * it is set up as a combination of 2 hidden macros `_CPI_CHECK` and `_QGL` to overcomes some Klipper macro variable quirk

* `[gcode_macro _CPI_CHECK]` and `[gcode_macro _QGL]`
  
  * see `[gcode_macro QUAD_GANTRY_LEVEL]`

* `[gcode_macro _ADJUST_Z_HOME_FOR_TOOL_OFFSET]`
  
  * used in the homing routine to account for the z-offset of the toolhead

* `[gcode_macro _APPLY_ACTIVE_TOOL_GCODE_OFFSETS]`
  
  * used in the homing routine to account for the gcode offsets of the toolhead

* `[gcode_macro _TAP_PROBE_ACTIVATE]`
  
  * used in each toolhead config to ensure safe temp for bed probing

* `[gcode_macro _INITIALIZE_FROM_DETECTED_TOOL]`
  
  * to initialise from detected tool

* `[gcode_macro PRINTER_CONFIG_TOGGLE]`
  
  * to toggle between config with dock (tool changer) and without dock (single toolhead).
  
  * This macro require `[config_switch]` and session variables to be present in `printer.cfg` 

### printer.cfg

This file is FOR REFERENCE ONLY.

It is shows the recommended layout of the `printer.cfg` file. More specifically, what and where the session variables are meant to be placed in the config.

### T*.cfg

These file are FOR REFERENCE ONLY.

Note for `T0-SB2209-Revo-LDO.cfg` :

* It is required by Klipper for the `[extruder]` object to exist. However, for the sub-sequence toolheads other names can used, i.e. `extruder1`, etc.

* As the reference toolhead gcode offsets of T0 are all 0.

* Parking parameters `params_park_*` will need to be manually calibrated

* Input shaper parameters will need to be manually calibrated

* <mark>BE MINDFUL</mark> of the `HEATER` parameter for `_TAP_PROBE_ACTIVATE`

Note for `T1-SB2209-Revo-LDO.cfg` :

* Parking parameters `params_park_*` will need to be manually calibrated

* Input shaper parameters will need to be manually calibrated

* <mark>BE MINDFUL</mark> of the `HEATER` parameter for `_TAP_PROBE_ACTIVATE`

### macro-*.cfg

These file are FOR REFERENCE ONLY.

Note for `macro-print.cfg` :

* It is advised to use, or at least study the component of, `PRINT_START` and `PRINT_END`
