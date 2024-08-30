# Material Test

This document aims to outline the lesson learned in trying to combine different materials in a print. It not an exhausted list of the characteristics of the materials, but a list of notes for what material can be combined and tips to get it done. 

## Summary

1. The shared resources for all toolhead are: chamber, bed, speed profile. This limits:
   
   * The material combination. In which, low temp filament cannot be used in a hot chamber. Also, the bed temperature have to be a compromised to fit the material being combine.
   
   * The print speed is limited by that of the lowest flow rate among the material, like PLA.

2. TBD

## General

1. PLA and PETG does not chemically stick to each other. This enable:
   
   * Use one to made the support for the other
   
   * Print-in-place hinges with very tight tolerance. Mechanism with wall-to-wall gap between the parts can go down to 0.15mm.
   
   However, there are challenges:
   
   * Because they don't stick to each others. The PETG bottom contact layer can sometime curled up and away from the support; PLA bottom contact layer is not affected.

2. The wipe tower needs to be configured for non-chemically compatible:
   
   * The `Wipe tower extruder:` setting (which set the material of the wipe tower shell) should to be set to the toolhead that is responsible for the more stringy material. For example, if you are printing PLA and PETG, you will need to set the tower shell material to PETG. This will allows the material more time and extrusion pressure to stabilise at the tip of the nozzle, without increasing the size and material usage for the wipe tower.

3. The heated chamber that ABS like to have, ~55°C, will cause PLA and PETG to soften and clog in the extruder. This suggest a limitation to a tool-changer system. Even though the different tool-head allows different materials to be printed together, all working materials are still subjected to the same build environment (i.e. chamber and bed). Therefore, there is a limit to what material can be mixed in a print; regardless of their chemical compatibility.

4. There is no logic to what the bed temp should be for a multi-material print in the slicer. It just pick that of the first material being extruded.
   
   This is raised an interesting point, as the normal bed temp for PETG, ~85°C, is too hot for PLA and that of PLA, ~55°C is too low for PETG.
   
   The solution for PLA and PETG is to have the material profiles for them in the slicer, which specifically for them to work with each other, with the same the bed temp at ~65°C. However, what about ASA, with the bed temp at ~100°C? - The 100°C bed of ABS will cause PETG to permanent marks the bed... don't ask :(
   
   The implication of this problem is that only material of similar bed temperature parameters can be printed together, in general. However, it is to be tested whether this limitation can be overcomes by having a draft layer of a single material.

## PLA

1. This is the easiest material to work with.

2. PLA works very well as a support material for PETG. It does not curl (wrap) during print, form fine strings.

3. The speed profile for PLA is difference than that of PETG and other material. The max flow rate is not as high.

## PETG

1. PETG has a very high tendency to string. this leads to:
   
   * Build up around the nozzle over time. this blob of material would often fails off and get locked into the parts.
   
   * The strings that came out during tool-change can get locked into the adjacent part, either mechanically or chemically.
   
   Mitigation 1, cleaning. The specific toolhead for PETG will need a macro to periodically wipe the nozzle mid-print. Or, toolchanger designers can integrate nozzle brushes into path of each tool change.
   
   Mitigation 2, wipe tower. In Prusa Slicer, under `Print Settings` > `Multiple Extruders` > `Extruders` there are options to select a specific extruder for a task.

2. This material is not ideal for internal parts, where the nozzle need to pass over another part, regardless of the material of that other part. as the strings coming off the nozzle will be locked into the other items and potentially weaken them, or lock up the interface between them.

## ABS

1. This material cannot be printed with PLA or PETG, as the chamber temperature is will cause PLA and PETG to soften and clog in the extruder.
