# Configs Descriptions

## config (folder)

This folder is an auto-generated folder, that contains the configs that is under `printer.cfg`'s `Session Variables`. It is generated when the gcode_macro `A_CONFIG_TOGGLE` is triggered.

## macro-general.cfg

This contains a collection of macros that are not essential. However, you may find then useful for the making of your own macros. Nevertheless, it is worth noting that none of of the macros in this file is expected to work correct with your printer.

## macro-print.cfg

This config contains the macros that are relevant to running a print, i.e. `PRINT_START`, `PRINT_END`, etc. There are many setting that may not relevant to your printer. Thus, it is advised that you read through these macros and remove (or comment out) what is not applicable to your set up. 

Functions in `[gcode_macro PRINT_START]` / `[gcode_macro PRINT_END]` that may not be relevant or wanted on your printer:

* Bed Mesh - adaptive mesh for every print

* Clean off the initial ooze - the function for this is in `toolchanger-nozzle_clean.cfg` and it need the cleaning dock to be installed. It can be disabled by setting `variable_clean_dock_x` to "0", in the `[gcode_macro _global_variable]` in `printer.cfg` 

* Skew - This is a recommended tuning step that is done with the [Calilantern Calibration Tool](https://vector3d.shop/products/calilantern-calibration). Nevertheless, it can be disabled by setting `variable_skew_compensation` to "0", in the `[gcode_macro _global_variable]` in `printer.cfg`

* Account for frame thermal expansion - Require chamber thermistor. It can be disabled by setting `variable_thermo_expand_temp_low` to "0", in the `[gcode_macro _global_variable]` in `printer.cfg`

## printer.cfg

The `printer.cfg` is a requirement for Klipper, and is the file that the system looks at on boot. The `printer.cfg` of the reference machine is included here as reference only. There are many settings that will not be correct for your set up.

## T0.cfg

Reference for T0

## T1.cfg / T2.cfg

Reference for T1, T2, etc.

## toolchanger.cfg

### [toolchanger]

This function controls the tool changing routine. It is developed by [viesturz](https://github.com/viesturz/klipper-toolchanger/blob/main/toolchanger.md) who has outlined all for setting for it in his repository. Nevertheless, here is where custom drop-off and pick-up gcodes can be set.

 The setting that is most important for MissChanger users are:

```
params_safe_y: 120           # The closest toward the front that the toolhead can go without coliding with the docked toolhead(s)
params_close_y: 50           # The closest toward the front that the toolhead-carrier can go without coliding with the docked toolhead(s)
params_min_z: 40             # The lowest z position that tool-changing can happen
params_max_z: 240            # The highest z position that tool-changing can happen
params_fast_speed: 30000     # (1500 = 40mm/s) The speed to travel to the first point in the tool-change path
params_path_speed: 7000      # (1500 = 40mm/s) The max speed to travel during the tool-change path
```

These parameters are that of the reference printer. However, it is advised that you double check if they will works with your specific setup.

In addition there is also:

```
params_input_shaper_type_x: 'ei'
params_input_shaper_freq_x: 47.200
params_input_shaper_damping_ratio_x: 0.100000
params_input_shaper_type_y: 'ei'
params_input_shaper_freq_y: 34.800
params_input_shaper_damping_ratio_y: 0.100000
```

These are the input shaping variables that are given in the console after `SHAPER_CALIBRATE`. It is used if the initialised tool-head does not have it's IS values specified. The specific value here is depended on your setup. The best way to determine them is to take the average of that of all your tool-heads.

In addition, the function will use:

* `_CLEAN_MID_PRINT` in `toolchanger-nozzle_clean.cfg` 

* `[gcode_macro _global_variable]` in `printer.cfg` 

* `[tool Tx]` in each of the tool-head config

### [tool_probe_endstop]

This function define the steps that will be done if the system does not detect a tool-head when there should be one mounted to the gantry.

### Marcos overwrite

There are rewrites in `toolchanger.cfg` for:

- `M104` / `M109` - for tool-head heating

- `BED_MESH_CALIBRATE`

- `QUAD_GANTRY_LEVEL`

to account for the addition tool-heads.

### Hidden

Hidden functions that are relevant to tool-changing includes:

* `_CPI_CHECK`- check the state of the Nudge calibration probe

* `_QGL` - override the default QGL. It is done as such to overcomes some Klipper macro variable quirks

* `_ADJUST_Z_HOME_FOR_TOOL_OFFSET` - Account for the tool's z-offset during home

* `_APPLY_ACTIVE_TOOL_GCODE_OFFSETS` - Apply the tool offsets (relative to T0) after tool-change

* `_TAP_PROBE_ACTIVATE` - Prevent the tool from probing if the nozzle is above 150°C

* `_INITIALIZE_FROM_DETECTED_TOOL` - Initialised from detected tool

### Working Functions

A macro to toggle the config file between with or without dock, `[gcode_macro A_CONFIG_TOGGLE]`. This macro will:

* Save the current config

* Toggle between the config with-dock and without-dock

* `FIRMWARE_RESTART` to apply the change

## toolchanger-calibrate_offsets.cfg

### [tools_calibrate]

This function is for the automated routine to calibrate the tool-head, relative to T0, using the Nudge probe. The most important setting for your particular system is `trigger_to_bottom_z`. It is used for iterating the z-offset reading for all tool-head; and may needed to be adjusted several times.

### Hidden

Hidden macros are used here to overcomes some Klipper macro variable quirks. They defines the series of step to do perform before `TOOL_CALIBRATE_TOOL_OFFSET` and/or `TOOL_CALIBRATE_PROBE_OFFSET`.

### Working

The macro in this section use the `_CPI_CHECK` macro in `toolchanger.cfg`, the `[gcode_macro _global_variable]` and `[gcode_macro _home]` in `printer.cfg`.

* `[gcode_macro CALIBRATE_OFFSETS]` - runs the routine to calibrate all relevant offsets includes:
  
  1. Check for homed gantry
  
  2. Check for Quad Gantry Level
  
  3. Check for an enabled calibration probe
  
  4. Pick up T0
  
  5. Move to the appropriated position of the probe - Define in the global variable, in `printer.cfg`
  
  6. Identify the exact position of the probe using T0
  
  7. Calibrate the absolute z-offset of T0
  
  8. Pick up the next tool
  
  9. Calibrate the offsets, relative to T0
  
  10. Calibrate absolute z-offset of the tool-head (probe offset)
  
  11. Repeat step 8+, until the last tool head
  
  12. Pick up T0, and go to the centre of the bed
  
  Note: It is possible to calibrate a specific tool-head, but step 1 to 7 will still be done each time to locate the probe.

* `[gcode_macro CALIBRATE_NPO]` - Similar to `CALIBRATE_OFFSETS` this macro will locate the probe using T0. However, only the absolute z-offset of the current too-head (the tool-head that is mounted before starting the macro) is measured.

## toolchanger-homing.cfg

The function `[force_move]` and `[homing_override]` are used to override the default homing sequence, to allow the printer to home Y before X. This reduces the risk of collision between the tool-heads and allows the x endstop to be mounted at the back right corner. In addition, it also programmed with extra moves to avoid potential collision between the umbilical / between the nozzle and the bed (or the calibration Nudge probe).

The `[homing_override]` function will use variables from `[gcode_macro _home]` in `printer.cfg`.

## toolchanger-nozzle_clean.cfg

The macro in this section use the `[gcode_macro _global_variable]` and `[gcode_macro _home]` in `printer.cfg`.

### Hidden

* `[gcode_macro _SCRUB]` - The scrubbing routine, shared between all cleaning macro

* `[gcode_macro _CLEAN]` - scrubbing at progressively lower temperatures and kept probe crash detection disabled

* `[gcode_macro _CLEAN_MID_PRINT]` - Scrub once and turn on probe crash detection at the end

### Working

* `[gcode_macro CLEAN_NOZZLE]` - This macro give the option to `_CLEAN` a specific nozzle or `_CLEAN` all nozzles
