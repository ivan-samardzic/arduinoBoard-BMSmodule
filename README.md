### _Arduino Battery Management System Extension Module_

This _Arduino BMS Extension Board_ can be controlled with _Nano_, _Uno_ and other Arduino Microcontroller boards.

The goal of this project is to design, sketch and program a pcb board that will be used as an extension board for Arduino microcontroller. 

This board has a function to measure battery voltage and temperature in an analog form and send it back to the microcontroller. In microcontroller conversion is executed and
depending on the BAT temperature and voltage values, system will turn on/off the N-channel MOSFET and flash the LED with proper pattern. The board also generate stable 3.3 V (600 mA) 
output signal by using appropriate integrated circuit thus eliminating the need for an external voltage source for the board and the microcontroller. 

### Recommended to view
[Battery management system explanation video tutorial](https://www.youtube.com/watch?v=MZyY1dpka7c)

[Full Arduino Uno schematic](https://www.circuito.io/blog/arduino-uno-pinout/)

[TPS61073 IC data](https://www.ti.com/lit/ds/symlink/tps61073.pdf?ts=1597321829455)

[B5789 NTC data](https://product.tdk.com/en/search/sensor/ntc/ntc_element/info?part_no=B57861S0103A039)

### Problem

A DIY Powerwall is the DIY construction of a pack of battery cells to create an energy store which can be used via inverters to power electrical items in the home. Generally cells are salvaged/second hand, and typically use Lithium 18650 cells.

Lithium batteries need to be kept at the same voltage level across a parallel pack. This is done by balancing each cell in the pack to raise or lower its voltage to match the others.

Existing balancing solutions are available in the market place, but at a relatively high cost compared to the cost of the battery bank, so this project is to design a low-cost, simple featured BMS/balancer.

A large number of people have utilised the commercial BATRIUM BMS system in their powerwall devices.

### Scheme explanation

The schematic attached in this repository is very simple. 

Electronic circuit can be divided in two parts:
  1. Power supply module
  2. Analog to digital conversion peripheral 

First part has the a function of generating a stable voltage to the ADC peripheral (main IC) always has the same voltage reference. Power supply module is made by using TPS6107x
integrated circuit (buck/boost converter IC). Looking in the IC datasheet, there will be represented some examples how to use this integrated circuit as well as schematics for that 
purposes. 

The TPS6107x devices provide a power supply solution for products powered by either a one-cell, two-cell, or three-cell alkaline, NiCd or NiMH, or one cell Li-ion or Li-polymer
battery. In this example, TPSxx will be used as both buck and boost converter. It gives a stable 3.3 V (600 mA) signal at its output while at its input, voltage value varies
in range from 3 V to 4.2 V in case system manages Li-Ion battery cells.

In different modes, integrated circuit has the different efficiency. Module has cca. 80 percent efficiency in the boost mode and over 90 percent in its buck mode. Power supply is
connected directly to batteries meaning that we do not need additional external power source.

All 4.7 uF capacitors connceted to TPS6107x chip are used to give the maximum power at the output. Two 4.7 uF X7R capacitors instead of one 10uF capacitor are better solution due to
less value noise generation.

The output voltage of the TPS6107x dc/dc converter can be adjusted with an external resistor divider. The typical value of the voltage at the FB pin is 500 mV. The maximum
recommended value for the output voltage is 5.5 V. The current through the resistive divider should be about 100 times greater than the current into the FB pin.

The typical current into the FB pin is 0.01 µA, and the voltage across R2 is typically 500 mV. Based on those two values, the recommended value for R2 should be lower than 500 kΩ, in
order to set the divider current at 1 µA or higher. Because of internal compensation circuitry, the value for this resistor should be in the range of 200 kΩ. The second parameter for
choosing the inductor is the desired current ripple in the inductor. 

Normally, it is advisable to work with a ripple of less than 20% of the average inductor current. A smaller ripple reduces the magnetic hysteresis losses in the inductor, as well as
output voltage ripple and EMI. With this calculated value and the calculated currents, it is possible to choose a suitable inductor. In typical applications, a 4.7-µH inductance is
recommended. The device has been optimized to operate with inductance values between 2.2 µH and 10 µH.

ADC peripheral module is in charge of measuring battery voltage and temperature temporary values. For voltage measurements, voltage divides is used to divide maximal 4.3 V value to 
2.4 V value due to ADC characteristics. Divide ratio equals 680 / 680 + (470 + 47) =  0.568. Also, a small 100 nF capacitor is added to integrate the battery voltages.

For thermal measurements, 10kΩ NTC thermistor is used and together with 10 kΩ resistor make voltage divider in which voltage is raised when thr temperature is raised due to divider
configuration.


### KiCad Design Tool

_PCB Editor photo_

![GitHub Logo](/BMS-Design/arduinoUno-BMSmodule.png)

_3D View photo_

![GitHub Logo](/BMS-Design/arduinoUno-BMSmodule2.png)

### Required components

U1 TPS61073 Converter 

Q1 BS2312-BDS N-channel MOSFET

TH1 B5789 NTC Thermistor

R1, R3 10 k Resistor

R2, R5 47k Resistor

R4 10 W 33 R Power Resistor

R6 470k Resistor

R7 680k Resistor

R8 1M Resistor

R9 200k Resistor

R10 330R Resistor

L1 4.7 uH Inductor

C1 X7R 100 nF Ceramic Capacitor

C2 - C5  X7R 4.7 uF Ceramic Capacitor

C6 100 nF Electrolitic Capacitor

D1 - D2 5 mm LEDs

J1 01x02 5.08 mm Male Header Connector

J2 - J8 01x02 2.54mm Male Header Connector
