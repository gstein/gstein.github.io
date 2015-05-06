---
title: Microcontroller-based board for running high-voltage/current LEDs using PWM.
labels: PWM,Eagle,PIC,Microcontroller,LED,MOSFET
---

# Introduction

This board is used to control 9 channels of high-voltage and/or high-current. Nominally, the channels will be used for LEDs, and the microcontroller will use PWM for dimming capability.

However, any non-inductive load would be just fine, so you could see this as a 9-channel power control board.

*Warning*: I have not analyzed whether this board can deal with reverse voltages caused by inductive fields. The MOSFETs that I have selected have *some* protection. If you intend to run (say) motors using this, then external circuitry may be required.

*Note*: you may have arrived here via http://goo.gl/b0P8Q6, a short link silked onto the board. This page describes what you are holding. See the [PWMBoard#Errata Errata], for any discovered problems.

*Note*: wiki-comments are enabled below, any questions or feedback are most welcome.

# Details

The board is focused around a PIC16F688 microcontroller. It speaks "upstream" via the RX/TX pins, allowing for TTL Serial or I2C via bit-banging.

The 9 outputs of the PIC control MOSFETs which are set up as low-side drivers (we sink/control current to ground). Note this board only requires supply voltage for the PIC, since the target loads' +V supply is off-board. Their ground *is* shared (splitting ground would need a design tweak to introduce optoisolators).

Each channel can carry about 4.7A maximum, based on the board's 100 mil traces and a standard 1oz copper thickness. As a whole, the board can sink a total of 8.7A based on the larger 234 mil traces leading to the ground supply, meaning 0.97A continuous for all channels simultaneously. The MOSFETs are generously-spaced in case heat sinks are required.

Note that you could use a 2oz copper pour on the PCB to double the current capacity, but beware of the power dissipation capability of these MOSFETs. If you're going to drive them hard, they'll need good heat sinks. Alternatively, you could easily replace the part with something more capable, but make sure to select a logic level gate MOSFET that the PIC can control.

The PIC can run with a supply voltage up to +6.5V. The higher, the better, in order to quickly switch the MOSFET on/off, and get more current through. I plan to run this board using standard +5V, though it could easily go with +3.3V depending upon your upstream controller and what is coming across the RX/TX pins.

## Eagle Files

http://gstein.googlecode.com/svn/trunk/eagle/PWMBoard/

<font size="2" color="#404040">(yes, I know the ground trace on the "bottom" is wonky; it is very difficult in Eagle to have a GND signal that uses different Net Classes (and thus, trace widths); I forced it to work, and haven't bothered to clean it up)</font>

## Controller Code

In my project, I'm using I2C for the communication bus, and PWM for 12V and 24V LED lighting. The code is located at:

_this section is still TBD_

Some pieces still in development:
* http://gstein.googlecode.com/svn/trunk/pic/pwm-board/
* http://gstein.googlecode.com/svn/trunk/pic/pwm/
* http://gstein.googlecode.com/svn/trunk/pic/i2c-sync/

## Parts List (Bill of Materials)

* PIC16F688 (Mouser #579-PIC16F688-I/P; !SparkFun COM-00219)
* 14-pin DIP socket (recommended) (Mouser #571-1-390261-3; !SparkFun PRT-07939)
* 10k resistor array (Mouser #652-4610X-1LF-10K)
* (9) N-Channel MOSFETs (Mouser #512-FQP30N06L; !SparkFun COM-10213)
* (3) 3-position screw terminals (Mouser #651-1935174; !SparkFun PRT-08433)
* (2) 2-position screw terminals (Mouser #571-1546216-2; !SparkFun PRT-08432)
* 0.1 µF capacitor

Approximate cost: US$12.68, including PCB, and excluding shipping (note: consumer-level part quantities)

## Power Consumption

The power through your MOSFETs is (obviously) based on the loads you have connected.

On the PIC "side" of the board, the power consumption is will be based on current through the pull-down resistors, when a channel is turned on. With a 5V supply and 10k pull-downs, this will be 0.5 mA per channel, or 4.5 mA when all channels are enabled. The PIC will consume about 2 mA maximum.

# Errata

Each board has a revision number silked onto it. See below for details about any problems discovered.

## Revision: 224

This revision of the board connected RA3 of the PIC to channel 1b. However, RA3 is *input only*, so it cannot actually drive that channel. The solution is to jumper the (unconnected) RC2 over to RA3, and drive the channel using RC2 instead.
