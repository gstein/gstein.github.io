---
title: Details of the Wall Switch Software
---
# Wall Switch Software

The WallSwitch has a PIC16F688 microcontroller to manage all of its functions.
This page details the basic organization and the various submodules.

_(see WallSwitch for more detail on the hardware and its components)_

The software is located in the [http://gstein.googlecode.com/svn/trunk/pic/ PIC area of my repository].

## I2C

Communication from the switch to the main house controller, and to the downstream wall switches, is done via an I2C bus. These are enhance with drivers to overcome the extra capacitance due to the distances involved.

The PIC in the switch needs to act as an I2C slave, responding to the main controller. It also needs to act as an I2C master to talk to the main controller and to the downstream MPR121 devices.

The 16F688 does *not* have builtin I2C capability, so this work will be done by "bit-banging".


## RS232

The optional ePIR sensor connected to the control board uses RS232 for communication. The 16F688 has a built in UART, so much of this code will be quite easy.

## TMP36

Reading a temperature from the TMP36 is performed with a simple A/D conversion to read the current voltage produced by the part.

## WS2801

The RGB LEDs are managed with a WS2801. The PIC needs code to clock new RGB values into the (chain) of WS2801 controllers.
