# Material Test

This document aims to outline the lesson learned in trying to combine different materials in a print. It not an exhausted list of the characteristics of the materials, but a list of notes for what material can be combined and tips to get it done. 

## General Information

1. The shared resources for all toolhead are: chamber, bed, speed profile. This limits:
   
   - The material combination. In which, low temp filament cannot be used in a hot chamber. The heated chamber that ABS like to have, 40+°C, will cause PLA and PETG to soften and clog in the extruder.
   
   - The bed temperature have to be a compromised value to fit the materials being combined.
   
   - The print speed is limited by that of the lowest flow rate among the material
   
   - Materials have different preference for z-offset (on the same toolhead) depending on whether it need a enclosure or not, due to thermal expansion of the frame.

2. Due to the order of tool changes in Prusa Slicer, a "PAUSE" operation might happen before the expected layer. This is because the slicer aims to minimise the number of tool changes, it will print the next layer with the currently initialised toolhead first, before tool-change.

3. There is no logic to what the bed temp should be for a multi-material print in the slicer. It just pick that of the first material being extruded.

## Combination Matrix

|      | ABS | ASA | PETG | PET | PLA | TPU |
| ---- | --- | --- | ---- | --- | --- | --- |
| ABS  | n/a |     |      |     |     |     |
| ASA  |     | n/a |      |     |     |     |
| PETG |     | 3   | n/a  |     |     |     |
| PET  |     |     |      | n/a |     |     |
| PLA  | 2   |     | 1    |     | n/a |     |
| TPU  |     |     |      |     |     | n/a |

### 1 - PLA and PETG

* These two materials are the most accessible material for 3D printing, they can easily be found at very affordable prices around the world.
- PLA and PETG does not chemically stick to each other. This enable:
  
  - Use one to made the support for the other.
  
  - Print-in-place hinges with very tight tolerance. Mechanism with wall-to-wall gap between the parts can go down to 0.15mm. Further reduction in the tolerance can be achieved; however, below 0.15mm the coefficient of friction between the material will start taking it's tow.
* However, there are challenges. The wipe tower needs to be configured for non-chemically compatible:
  
  * The `Wipe tower extruder:` setting (which set the material of the wipe tower shell) should to be set to the toolhead that is responsible for the more stringy material. For example, if you are printing PLA and PETG, you will need to set the tower shell material to PETG. This will allows the material more time and extrusion pressure to stabilise at the tip of the nozzle, without increasing the size and material usage for the wipe tower.
  
  * There are limitations to the way that the wipe tower is done as it is now (09.2024). Since you can only specified a single nozzle as the `Wipe tower extruder:`, if you have 2 PETG nozzles (or any other stringie material) and at least 1 PLA nozzle (a material that is not compatible with the for-mentioned material), at least 1 of the PETG nozzle will have to deal with the risk of priming onto a layer of PLA. If the prime onto the PLA layer fails to adhere, the excess material will blob onto to the nozzle and leads to other problems.

* The normal bed temp for PETG, ~85°C, is too hot for PLA; and that of PLA, ~55°C, is too low for PETG. The solution is to have specific material profiles for the PLA+PETG prints with the same the bed temp at ~75°C, which will allow both to work reasonably well.

### 2 - PLA and ABS

* As shown by [JanTec Engineering](https://www.youtube.com/@JanTecEngineering) in [this  YouTube video](https://youtu.be/KnvEhYCimKc?si=OjUVotaZ15H8OHvi), PLA and ABS adhere relatively well with each other. Even though it is still not as good as ABS-ABS, PLA can still be served as the bottom interface layer for ABS parts. This open the door for PETG support for ABS parts.

* The need for a chamber and carbon filter for ABS printing is a large technical challenge to over comes. This is because PLA cannot be print in a chamber of 40°C or above.

## Material Specific

## PLA

1. This is the easiest material to work with.

2. It does not curl (warp) or form fine strings.

3. The speed profile for PLA is difference than that of PETG and other material. The max flow rate is not as high.

## PETG

1. PETG has a very high tendency to string. this leads to:
   
   * Build up around the nozzle over time. this blob of material would often fails off and get locked into the parts. This material is not ideal for internal parts
   
   * The strings that came out during tool-change can get mechanically locked into the adjacent part.
   
   * **Mitigation 1**, dry filament. This is more important for a tool-changer than it is for single nozzle prints.
   
   * **Mitigation 2**, cleaning. The PETG toolhead will benefit from a nozzle scrub between tool-change.
   
   * **Mitigation 3**, wipe tower. PETG will benefit from additional wipe distance.
   
   * **Mitigation 4**, idle temp. Set an idle temp for this material and enable the "Ooze Prevention" function in the slicer to mitigate the ooze build-up problem.

2. PETG oozing pressure is higher than other materials, partly due to how much moisture it can absorb. The nozzle plug will need to apply pressure to the nozzle to keep the material inside. This is unlike PLA and ABS, where no (or, very little) pressure is needed.

3. PETG lightly sticks to Teflon oven-liner, degrading it over time.
