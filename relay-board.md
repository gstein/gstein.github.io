---
title: PIC-based relays for high-voltage/power circuit control
labels: PIC,Microcontroller,Automation,Relay
---

# Introduction

Controlling high-voltage loads (eg. 110VAC lights) is a particular challenge for computing devices such as PCs, Arduinos, Raspberry PIs, and microcontrollers. There are inexpensive relay boards available that can map low-voltage/power to high-power/current relays.

The next problem is mapping a communication protocol to the control of these relay boards. I've chosen to use a PIC16F688 microcontroller to listen for commands over an I2C control bus, and manipulate the relay board.

# Details

On a 5cm x 7cm protoboard, I wired (3) '688 microcontrollers, each manipulating an 8-channel relay board. It is straightforward to add more '688 controllers and more relay boards when more channels are needed. Since each controller is individually addressable, any number can be placed onto the I2C data bus, paired with its own relay board.

## Code

The microcontroller code is located at:

http://gstein.googlecode.com/svn/trunk/pic/relay-board/

And its sibling directories.

## Schematic

_TBD_

(the schematic is straight-forward: wire 8 outputs to the relay board, 2 inputs for I2C, some power, and a capacitor across the power pins)

## Power Consumption

The PICs use just a couple milliamps maximum during operation.

The relay board is active *LOW*, so the microcontroller needs to sink about 2mA per channel to activate the optoisolator. The board has a separate supply circuit, which needs 60mA for each relay coil.

Note that the relay board is specified to operate at 5V. Many people are interfacing them with 3.3V controllers (eg. Raspberry Pi) and finding a need to tweak the board, or interpose a transistor to map between the 3.3 and 5 volt levels. The PIC16F688 easily operates at 5V, so no additional circuitry is necessary.


## Bill of Materials

* 8-channel relay board
* PIC16F688 (Mouser #579-PIC16F688-I/P; SparkFun COM-00219)
* 14-pin DIP socket (recommended) (Mouser #571-1-390261-3; SparkFun PRT-07939)
* (2) 2-position screw terminals (Mouser #571-1546216-2; SparkFun PRT-08432)
* 0.1 ÂµF capacitor
* protoboard
* jumper wires

The relay board can be purchased from Amazon (made by Sainsmart) or more cheaply via eBay. They are pretty universal, and just copies of each other. Look at the board design, pins, and electrical features.

## Example

Here is a picture of my installation, with (3) '688s each controlling an 8-channel relay board.

http://gstein.googlecode.com/svn/content/relay-control.jpg
