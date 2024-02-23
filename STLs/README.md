# PRINT INSTRUCTION

Infill: 40% ; Gyroid or Cubic

Walls: 4

No. top layers: 6

No. bottom layers: 5

No Support (unless noted otherwise below)

Disable "External perimeters first" (or equivalent option) in slicer. - Some of the parts don't like that setting.

All STLs are pre-orientated.

**Note:**

- The current Locker will need to be printed in the default orientation, with auto-painted support. (Built-in support will be add in later revisions)



# ASSEMBLING

## Endstops Assembly

The endstops assembly is held in place by replacing the original two M3x30mm bolts with two M3x35mm bolts, which passed through the holes on the Y motor mount assembly. 

The X endstop trigger is mounted to the back of the original "Tap_Center" using an M3x30mm bolt, which go through centre hole of the linear rail. There is a slot to add a M3 nut on the trigger.

**Note:** Do not insert the threaded insert for the centre hole, see picture below. Alternatively (if you are reusing parts from a functional Voron-Tap), the threading can be drilled out with a 3mm bit.

![20240218_221143.jpg](/images/20240218_221143.jpg)

![20240218_221143.jpg](/images/20240218_222328.jpg)

![20240218_221143.jpg](/images/20240218_223816.jpg)

## Tool Changer

The tool changer mechanism requires 3 modded pieces: the Tap Front, the Print-head Rear, and the Locker.

For the Print-head Rear. Two threaded inserts are needed. There is a insert slot for the pogo pins pad on the side (for the tap sensor). On the other side, are aligner-pin slots to mate with the dock; which will need 2 of 3mm sleeves. Finally, on the back, there are 2 of 3x20mm chamferred pins, which are pressed into place (they will butt up against the threaded 

The Locker required a M3x12mm bolt on the side, as shown in the picture below.

For the Tap Tront. There is the corresponding slot for the pogo pins pad. Then there are also aligner-pin slots for 4 of the 3mm sleeves (2 on each side).

**Note:** 
(1) the pogo pin pad on the print-head rear needs to be female, as the pins would be live during operation. 
(2) the chamfer on the pin should be increased (rounded if possible), for better mating action. 
(3) the locker is a consumable (although no cycle life time is available currently). Therefore, it is recommended to have at least 1 spare per toolhead.
(4) [reminder] the locker need to be printed with auto-painted supports, in the default orientation.
(5) The sleeves on the Tap Front can be pressed in from both sides. (2) You do not need to add the threaded inserts on to the tap upper, as shown.

![20240218_221143.jpg](/images/20240218_231424.jpg)

![20240218_221143.jpg](/images/20240218_231502.jpg)

![20240218_221143.jpg](/images/20240218_231551.jpg)

![20240223_151039.jpg](/images/20240223_151039.jpg)

![20240218_221143.jpg](/images/20240219_185221.jpg)

## Cabling

XLR connectors are used for easy connect/disconnect of the toolheads. 
However, they are not hot-plugs. The `printer.cfg` need to be edit and the printer need to be restart to add/remove toolheads.

Cable length from connector to toolhead is 780-800mm, with the Retractable Reel Clip cable tied  250mm from the end of the connector. The Reel Clip is hooked to the top with a M3 springed T-nut and a M3x16mm bolt.

Detail wiring. TBD.

PTFE tubes panel. TBD.

![20240223_155728.jpg](/images/20240223_155728.jpg)

![20240223_163258.jpg](/images/20240223_163258.jpg)

## Dock

TBD
