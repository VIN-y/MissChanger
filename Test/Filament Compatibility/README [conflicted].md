# Material Test

## General

1. PLA and PETG does not chemically stick to each other.

2. There is no logic to what the bed temp should be for a multi-material print in the slicer. It just pick that of the first material being extruded. 
   
   This is raised an interesting point, as the normal bed temp for PETG, ~85°C, is too hot for PLA and that of PLA, ~55°C is too low for PETG.
   
   The solution for PLA and PETG is to have the material profiles for them in the slicer, which specifically for them to work with each other, with the same the bed temp at ~70°C. However, what about ASA, with the bed temp at ~100°C? - The 100°C bed of ABS will cause PETG to permanent marks the bed... don't ask :(
   
   The implication of this problem is that only material of similar bed temperature parameters can be printed together, in general. However, it is to be tested whether this limitation can be overcomes by having a draft layer of a single material.

3. The heated chamber that ABS like to have, ~55°C, will cause PLA and PETG to soften and clog in the extruder.

## PLA

TBD

## PETG

1. PETG has a very high tendency to string. this leads to:
   
   * Build up around the nozzle over time. this blob of material would often fails off and get locked into the parts.
   
   * 
