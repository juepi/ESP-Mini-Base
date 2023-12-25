# ESP-Mini-Base
Base PCB ("hat") for ESP boards in WEMOS Mini format created with [KiCad EDA](https://www.kicad.org/). Primarily designed for [WEMOS S2 Mini](https://www.wemos.cc/en/latest/s2/s2_mini.html) and [D1 Mini](https://www.wemos.cc/en/latest/d1/d1_mini.html), might work for others / upcoming boards with a compatible pinout.  
The schematic describes the functionality and options in detail, it is also [available as PDF](pdf/ESP-Mini-Base_v1.0_schematic.pdf).

## Available functionality

The board provides the following functions:

* 3x N-Channel MOSFET drains (20A / 20Vds; no more than 5A recommended)
* 1x AC/DC SSR (48Vpp, 500mA max.)
* 1x Supply switch (5V or 3.3V, 2A max.)
* 2x 4-channel Level shifters to 5V
* 1x ADC attenuator with input selection (3V3 rail, 5V rail or external pin)
* 1x Pushbutton
* 3V3 and 5V I2C Pullups for default I2C pins of S2 and D1 Mini
* 5V LDO linear regulator for power supply up to 12V

The board has been designed in a way to allow you maximum flexibility. Every function needs to be enabled via solder jumpers on the bottom PCB side (for default wiring), or you may wire different IOs to the according pin headers on the board.  
The only exclusion are the 8 IOs wired to the 2 level shifters, which are hard wired and cannot be changed. The IOs have been selected to allow you level-shifted SPI or I2C for both S2 and D1 boards.

## Power Supply

There are several ways to power the board, the following table shows you the options and required configuration (solder jumpers on bottom PCB side):

| Powered by | Short Solder Jumpers | Notes |
| --- | --- | --- |
| 5V/VBUS Pin or USB Port | JP12 | Not recommended for MOSFET usage! At least connect GND wire to J9 if you try to do so|
| "12VDC" through J9 | JP12,JP13 | 5V generated from onboard regulator and supplied to ESP. Supported input voltage range: between 6.25 and 12V|
| 3.3VDC through J9 | none | Use to power ESP directly from LFP battery (in example)|

When powering the board from a LFP cell, you might want to have 5VDC (in example for a LED strip). Mainly for this reason, you have the ability to connect a Boost-converter board to J10. Short the Jumper JP11 and JP10 to 1-2 and you will be able to enable the Boost-converter via IO14 (U2 supply switch).  
Although the installed 5V LDO linear regulator supports up to 26V input voltage, no more than 12V are recommended due to the high power dissipation of the regulator.  

## Note on production data

The production data has been created for [JLCPCB assembly service](https://jlcpcb.com/). When placing an order, keep in mind that the rotation of several parts (MOSFETs, all ICs, C6) needs to be updated. Cross-check with the KiCAD Layout to verify that all parts will be placed correctly.

## Note on MOSFET usage

If you are switching a device through one of the 3 installed N-Channel MOSFETs, make sure that the switched device does **not have any reference to the input voltage GND** of the ESP-Mini-Base (in example through a data connection like I2C etc.). Chances are high to see magic smoke leaving your setup if you fail to use suitable galvanic isolation (beside the fact that the "switched" device probably won't power off at all)!
The MOSFETs have been tested fine with 5kHz PWM, so they should work for dimming RGB LED strips in example. Keep an eye on the overall current running through the FETs, more than 5A should be tested thoroughly before placing it into production.
  
## Pictures

![Assembled PCB, top side](pics/ESP-Mini-Base-v1.0_Top_Assembled.jpg?raw=true)
Assembled ESP-Mini-Base v1.0 top side with S2 Mini  

![Rendered PCB, bottom side](pics/ESP-Mini-Base-v1.0_Bot_rendered.jpg?raw=true)
Rendered view of the bottom side showing I/O and solder jumpers with descriptions.

**Note on I2C Pullup solder-jumpers (3,3 and 5V):** all 3 pins of the jumpers need to be connected with tin solder if you want to enable the pullups.

Have fun,  
Juergen