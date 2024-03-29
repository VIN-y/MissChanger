# MissChanger

A Stealth Burner tool-change system for Voron 2.4 and Trident.

This project was inspired by [tapchanger](https://github.com/viesturz/tapchanger/) and [Voron-Tap](https://github.com/VoronDesign/Voron-Tap/).

The endstops assembly is a remixing of the endstops carrier from [MrTeliP](https://www.printables.com/model/325765-voron-24r2-pg7-cable-gland-and-endstop) (on Printables).

![20240223_185152.jpg](/images/20240223_185152.jpg)

## Description

Misschanger aims to be a tool-changing mechanism that is compatible with both Voron Trident and Voron 2.4. While also retaining the MGN9H tapping system of Voron-Tap.

This tool-changer have been tested to work fine, printing at 200mm/s and 5000mm/s^2.

At the moment, it is only compatible with the Stealth Burner toolhead, with the Revo-Voron hotend, and the Voron 2.4.

## Assembly

Assembly steps are in the "STLs" folder.

The BOM only includes parts that are specific to this tool changer; and it is current a work-in-progress.

## Software

This project use the same [toolchanger extension](https://github.com/viesturz/klipper-toolchanger/) and Klipper setup as [tapchanger](https://github.com/viesturz/tapchanger/).
