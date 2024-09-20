# Assembly Guide

## 1. Tools

- Hex drivers

- Soldering iron (for the threaded inserts)

- Solder and heat shrinks

- Precision knife

- Metal ruler

- Pliers

## 2. Print parameters

All parts are designed to be print with the following parameters:

- All parts are pre-orientated in the STLs.

- 40% infill

- 4 walls

- ABS or ASA

- No support

## 3. Assembly

### 3.1. Dock

| Item                                           | Notes |
| ---------------------------------------------- | ----- |
| All printed parts in the [Dock](./Dock) folder |       |

#### Steps and note:

* Follow this video guide for the dock assembly: [MissChanger - Build Guide - Dock](https://youtu.be/sSsay7bBFj0)
* Follow this video guide for the dock calibration: [MissChanger - Build Guide - Dock Calibration](https://youtu.be/Xxpi4Nll_MY?si=lm0ChZX1qIrD1tWC)

### 3.2. XLR Panels

![](./images/XLR_panels.webp)

#### BOM:

| Item                                                       | Note |
| ---------------------------------------------------------- | ---- |
| All printed parts in the [XLR_Panels](./XLR_Panels) folder |      |

#### Steps and note:

- See the wiring diagram, back and basement view in section 3.5

### 3.3. Tool-head and Endstops Assembly

![](./images/tap&change_assembly.jpg)

![](./images/20240531_214047.jpg)

#### BOM:

| Item                                                                                  | Notes                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All printed parts in the [Tap&Change](./Tap&Change) folder                            |                                                                                                                                                                                                                                                                              |
| [Voron-Stealthburner](https://github.com/VoronDesign/Voron-Stealthburner) toolhead(s) | Due the complexity of the Stealburner and the number of variants it has, no stl. has been transferred to this repository. Please go to the official Voron repository for those files.<br/>Note: This guide will assumes that the Stealburner has been built and ready to go. |
| printed parts in the [SB22xx_&_CW2](./SB22xx_&_CW2) folder                            | only applicable if you are using the Clockwork 2 extruder and the SB22xx toolhead board                                                                                                                                                                                      |
| **PTFE_Panel** in the [others](./others) folder                                       |                                                                                                                                                                                                                                                                              |

#### Steps and note:

1. Preparation 1, the same as Voron Tap.

![](./images/rail_prepare_1.webp)

![](./images/rail_prepare_0.webp)

![](./images/magnets.jpg)

![](./images/built-in_support.webp)

![](./images/threaded_inserts.webp)

2. Preparation 2, other components.

![](./images/tap_center.webp)

![](./images/x-trigger.webp)

![](./images/tap&change1.webp)

![](./images/tap&change2.webp)

![](./images/tap&change3.webp)

![](./images/tap&change4.webp)

3. Assembly 1, the easy parts.

![](./images/endstop_ass.webp)

![](./images/center_ass.webp)

![](./images/tap&change_ass1.webp)

![](./images/tap&change_ass2.webp)

4.A note on belt tension. It is advised that you leave at least 15.5mm between the front idler slider and the idler body (see picture below). This is to give the maximum around of room for the front idler to travel before colliding with the dock's bar-ends.
![](./images/20240730_201311.jpg)

5. Assembly 2, the part that is too difficult to document in writing. Follow the video guides bellow.
   
   * Part 1: [MissChanger - Build Guide - Tool-head (part 1) - YouTube](https://youtu.be/jWHPMaVIBcA) 
   
   * Part 2: [MissChanger - Build Guide - Tool-head (part 2) - YouTube](https://youtu.be/t0auNNPhWSk) 

### 3.4. Calibration Probe

![](./images/20240605_232257.jpg)

#### BOM:

| Item                                                       | Notes                                               |
| ---------------------------------------------------------- | --------------------------------------------------- |
| All printed parts in the [Nudge](./Nudge) folder           |                                                     |
| [Nudge probe](https://github.com/zruncho3d/nudge) assembly | But, use this documentation for software and set up |

#### Steps and note:

* It is important to set up the software using the instruction in this repository (see the **README.md** in the parent folder of this one)

* Nudge assembly video: [Nudge Probe - Integrated Speedbuild](https://youtu.be/6eRomxUo7TI)

* Probe mount assembly:  [MissChanger - Build Guide - Nudge probe mount](https://youtu.be/ucKVRpfPakY)

### 3.5. Wiring

In this section, it is assumed that you have already have a working print, i.e. with the controller board connected and working. Therefore, no instruction will be given for the wiring of the controller, SBC, Z-axis motors, etc.

#### CAN bus tool-head wiring diagram:

![CAN_wiring](./images/CAN_wiring.jpg)

![](./images/basement.webp)

![](./images/z_chain_anchors.webp)

![](./images/back_view.webp)
