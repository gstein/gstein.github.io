---
title: Capacitive Sensor Wall Switch
---
# Capacitive Sensor Wall "Switch"

For my new house, I'm installing custom wall switches. These will be simple pieces
of glass covering a capacitive touch sensor.

# Features

* 10 sensor pad, enabling top/bottom/left/right/center taps, and vertical/horizontal swipes.
* RGB backlight for any color/level
* Optional temperature sensor
* Optional IR sensor


# Details

The focal point of this switch is an
[http://www.freescale.com/webapp/sps/site/prod_summary.jsp?code=MPR121 MPR121]
capacitive touch sensor IC from Freescale. It can run up to 12 sensors
and communicates over an I2C bus.

The MPR121 is a 3mm by 3mm 20 pin QFN package.
I don't have the tools to solder this onto my own (custom)
PCB, so I'm using [https://www.sparkfun.com/products/9695 a breakout board from SparkFun].

The MPR121 is managed by [http://www.microchip.com/wwwproducts/Devices.aspx?dDocName=en010215 PIC16F688 microcontroller]. The PIC also talks "upstream" to the main house controller, it talks to an optional [http://zilog.com/index.php?option=com_product&Itemid=26&task=parts&BL=147&familyId=159&productId=ZEPIR0BA Zilog ePIR sensor], to a [https://www.sparkfun.com/products/10988 TMP36 temperature sensor], and manages RGB LEDs to backlight the switches.

Up to four switches are managed by a single 16F688, due to the MPR121 only having four possible I2C addresses on the bus. The 16F688 will be omitted from the control boards of the "downstream" switches. Since this I2C bus runs for considerable distance between the switches (I2C is designed for low capacitance runs of a few feet), the bus is enhanced using a [http://www.linear.com/product/LTC4311 Linear LTC4311] to overcome the higher RC constant that affect the bus' switching time.

The RGB LEDs are managed by a [https://www.sparkfun.com/products/10444 WS2801 controller]. It does 3-channel PWM and constant current for the RGB LED. These are daisy-chained across the switches, allowing one PIC to manage multiple WS2801 controllers.

### Sensor Board

The sensor board is a simple two-layer board that will reside just under the glass plate. Copper pads on the PCB act as the capacitive sensors. Vias to the trace lines on the back of the board connect the pads to an SMT male header. We can't use a through-hole header since that would create protrusions on the glass side, holding the glass away from the board. The back side of the board also has some hatched ground-fill to prevent signals leaking to the pads/fields on the front side of the board.

I've committed the [http://gstein.googlecode.com/svn/trunk/eagle/SensorBoard/ Eagle files for the sensor], and sent the gerbers off for a small production run using the [http://imall.iteadstudio.com/open-pcb/pcb-prototyping/im120418002.html iTEAD prototyping PCB service]. I'll evaluate these, make fixes, and send out for the rest of the needed boards (I already know I forgot the drill holes to allow the LED backlight to shine through).

### Control Board

[http://gstein.googlecode.com/svn/trunk/eagle/ControlBoard/ Eagle files for the control board]

_more to come..._

### Capacitive Sensor Design

[http://cache.freescale.com/files/sensors/doc/app_note/AN3863.pdf Application note AN3863] was invaluable for assistance in designing the sensor board. If you want to modify the sensor in your own project, then keep that document handy.

### Physical Design

The target is a standard single-gang (US model) switch box. The glass can be anything, but translucent is best to hide the sensor pad. I'm going to be trying a white water glass, and have them cut to about 3.13" by 4.88" and pre-drilled to mount to the box.

The sensor board is 5cm x 10cm (to match iTEAD's service). The MPR121 breakout is stacked behind that, measuring 0.8" x 1.2". And finally behind that, the control board measures 1.5" x 3.25".

Power is received from the upstream main controller, and also delivered downstream through another link. A third possible cable leads to the (optional) ePIR sensor. Standard vertical-entry RJ45 connectors are used, to easily (dis)connect the switch from the cables coming into the switch box.

### Power

The switches will operate at 3.3V.

_TBD: power draw_

Each switch can be placed into a low-power state, consuming about 4mA each.

# Lighting

Note that on/off/dimming of the house lighting is performed by the main house controller. The switches merely provide the sensors.
