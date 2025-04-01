# Offset Precision Test

## 0. Revision and Update

### 16.05.25 - New Probe, New Software

Through long term testing, it has been shown that the Nudge probe, made with standard ABS and stainless steel screws are too sensitive for the use of MissChanger. More specifically, the ABS part would deforms over time in the heated chamber. In addition, the long stroke needed for the absolute z_offset calibration will throw the pin out of position; and, it will fail to return to the triggered state.

An alternative for MissChanger has been introduced, the Lubeballs probe. The STLs for Lubeballs is included from v1.2 onward. However, the Nudge probe remains to be the recommended probe for Trident builds, due to the lack of a mechanical probe mount on that platform.

## 1. Background

Calibration is the one of the most importance step to be done once the printer is built. For a tool-changer is a daunting task, as not only there are more nozzle whose z-offset needs to be calibrated, there is also the need to calibrate the XYZ offsets between every tool-head, against the reference (T0).

For the purpose of MissChanger, the [Nudge](https://github.com/zruncho3d/nudge) (made by [zruncho3d](https://github.com/zruncho3d)) probe have been used to automate the calibration for all of the offsets above. This hardware was chosen for its simplicity, affordability and high repeat-ability (± 0.01 mm).

To control the hardware, MissChanger has utilised Viesturz's [tools_calibrate.py](https://github.com/viesturz/klipper-toolchanger/blob/main/klipper/extras/tools_calibrate.py) code, which is available as part of his [klipper-toolchanger](https://github.com/viesturz/klipper-toolchanger) extension. However, unlike the hardware, the software is still in active development, with missing basic features and no documentation. There is still a long way to go before the software extension is ready for the mass.  Further testings and refinements are needed.

## 2. Equipment

### 2.1. Software Versions

| Name                | Version     |
| ------------------- | ----------- |
| klipper             | v0.12.0-132 |
| klipper-toolchanger | v0.0.0-69   |

### 2.2 Relevance Hardware

- Revo Voron

- MissChanger v1.0

## 3. Scope

This test aims to:

1. Establish the degree of which some parameters influence the accuracy and precision of the offset calibration.

2. Create a calibration procedure that is the best fit for MissChanger.

*Note: As I do not have access to other tool-changer systems. It cannot be guaranteed that resulted procedure can be used successfully with those system. However, it is my hope that the lesson learned here can still be useful for them.*

## 4. System Description

### 4.1. Machine parameters

The limits of the tested printer are:

```
[printer]
kinematics: corexy           # Printer type：corexy
max_velocity: 300            # Maximum speed (max. 300)
max_accel: 5000              # Maximum acceleration (max. 4000)
max_z_velocity: 15           # Z-axis maximum speed
max_z_accel: 350             # Z-axis maximum acceleration
square_corner_velocity: 5.0  # Square corner speed
```

And, the relevant parameters of the tool-changes are:

```
[toolchanger]
params_fast_speed: 9000
params_path_speed: 5000
```

### 4.2. Software

There are 2 component to the software:

1. The back-end python code

2. The user configurable `calibrate-offsets.cfg` file.

#### 4.2.1. Back-end code

The back-end python code provides the essential routines for the calibration process, which are:

* `TOOL_LOCATE_SENSOR` - determine the exact position of the Nudge probe.

* `TOOL_CALIBRATE_TOOL_OFFSET` - calibrate the tool offset.

* `TOOL_CALIBRATE_PROBE_OFFSET` - calibrate the z probe offset.

*Note: As these routines by-pass Klipper's safety system, it is not advisable for anyone that is not a developer to tinker with the python code, as it can cause serious damages. In addition, these routines only cover the specific set of motions that probes the calibration pin.*

#### 4.2.2. Configuration

**Update: These calibration commands are no longer functional, due to the software stack revamping of v1.2. However, the result of the analysis is the same.**

To provide flexibility and ease of use, some variables are exposed to the user which can be configured under `[tool_calibrate]`,  in the `calibrate-offsets.cfg`.

Furthermore, the user will need to define the approximate position of the  probe in the hidden macro. `[gcode_macro _CALIBRATE_MOVE_OVER_PROBE]`. 

Last, but not least, are the macros: `CALIBRATE_OFFSETS` and `CALIBRATE_NPO`; which are used for configuring the full calibration routine for all offsets and (specifically) the probe offset, respectively. These macros are adapted from Viesturz's sample macros: `CALIBRATE_TOOL_OFFSETS` and `CALIBRATE_NOZZLE_PROBE_OFFSET`.

## 5. Test

### 5.1. Procedure

**Before the test:**

1. Home all

2. Quad-gantry level

3. Heat soak the chamber; if applicable

**The procedure for each run:**

1. Home all axis

2. Run `TEST_TOOL_DOCKING`; if applicable

3. Run `CALIBRATE_NPO`

*Note: The experiment was carried out several times to refine the procedure and verify the the accuracy.*

### 5.2. Test case

|        | Nozzle heating | Chamber heating | Tool Change |
|:------:|:--------------:|:---------------:|:-----------:|
| **A1** |                |                 |             |
| **A2** |                |                 | yes         |
| **B1** |                | yes             |             |
| **B2** |                | yes             | yes         |
| **C1** | yes            |                 |             |
| **C2** | yes            |                 | yes         |
| **D1** | yes            | yes             |             |
| **D2** | yes            | yes             | yes         |

## 6. Results

### 6.1. Data

| Condition | Try 1  | Try 2  | Try 3  | Try 4  | Try 5  | Average | Standard Deviation |
|:---------:|:------:|:------:|:------:|:------:|:------:|:-------:|:------------------:|
| A1        | -1.065 | -1.056 | -1.060 | -1.065 | -1.063 | -1.0618 | 0.0038             |
| A2        | -1.056 | -1.058 | -1.061 | -1.065 | -1.063 | -1.0606 | 0.0036             |
| B1        | -0.916 | -0.913 | -0.914 | -0.918 | -0.915 | -0.9152 | 0.0019             |
| B2        | -0.923 | -0.930 | -0.925 | -0.926 | -0.930 | -0.9268 | 0.0031             |
| C1        | -1.045 | -1.030 | -1.025 | -1.031 | -1.026 | -1.0314 | 0.0080             |
| C2        | -1.028 | -1.021 | -1.028 | -1.023 | -1.018 | -1.0236 | 0.0044             |
| D1        | -0.920 | -0.913 | -0.913 | -0.920 | -0.918 | -0.9168 | 0.0036             |
| D2        | -0.925 | -0.928 | -0.933 | -0.926 | -0.916 | -0.9256 | 0.0062             |

*TC = Tool Change

![](./Nudge%20Probe%20Precision%20Test.png)

### 6.2 Discussion

From data collected above, the following points can be made:

Heat soaking the chamber has the most significant impact on the probe-offset. Comparing data from Bs and Ds with As and Cs.

Nozzle heating also have a notable impact on the probe-offset; unless, the chamber is heat soaked.

Nozzle heating introduces significant error to the calibration process. Comparing data from As and Bs with Cs and Ds.

Tool change using MissChanger, resulted in small but consistence errors in the probe offset in any heated condition. However, the variation are sufficiently small (~ 0.01 mm at most, between the average of case B1 and B2). Thus, it is not expected to be significantly impact print quality. Nevertheless, the most accurate results for tool calibration is when no heater is on, see the different between case A1 and A2, suggesting that this is best condition for run the calibration of multiple tool-head. calibration in this condition does not take into consideration of thermal expansion. However, that can be dial into the `trigger_to_bottom_z` under `[tools_calibrate]` in `calibrate-offsets.cfg`.

## 7. Conclusion

From the result and discuss above, it can be concluded the best calibration condition for the equipment outlined in section 2 is that of a cold bed and nozzles.
