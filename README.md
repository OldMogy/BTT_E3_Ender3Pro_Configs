
## My personal Marlin configuration files that I use with my Ender 3 Pro (direct drive)
Hardware
- BTT E3 Turbo Board
- NeoPixels enabled
- Inductive ABL Sensor enabled
- Mainboard temperature sensor enabled
- Hot End fan is controlled through the E1 port. This enables the fan to 
  turn off/on at desired temperature (I use default 50degC)
- Controller Fan is also controlled from PWM fan output, but is enabled to come on at 30degC  

### Marlin Bugfix 2.0.x Configuration Files.
Pulled initally from Marlin repository on Jan 21 2021 - Bugfix branch. This fixes the issue with the E3 Turbo board failing with EEPROM errors after reflashing with modified BTT firware branch.

The Neopixels I use are powered from a separate 5V regulator (20) and are setup to 
flashing at startup, and indicate heating status of printer, then turn white when printing.
Basically I run a single wire from the P1_24 (1.24) pin to the Nexopixel strip, the GND/5V from regulator to the Neopixel strip. Take note that the GND of regulator is directly connected to GND of E3 Turbo board.


My inductive sensor is a [J12A3-4-Z/BY PNP DC6-36V Inductive Proximity Sensor](https://www.banggood.com/LJ12A3-4-Z-or-BY-PNP-DC6-36V-Inductive-Proximity-Sensor-Detection-Switch-p-982679.html?rmmds=myorder) from Bangood (~$7AUD)
I run it from 24V Ender 3 Pro supply, then use simple voltage divider (20k/5k) on the output pin to get a voltage of approx 4.7 volts. This is then used to drive the gate of a small signal mosfet (2N7000). The drain is connected to end stop signal pin, and the collector is grounded. This only works because the internal circuitry is suitable (10k pullup). If you do this, make sure you are well aware of how it works before potentially blowing up your board.

I use the onboard 100k thermistor to monitor the temperature within the printer controller. Its enabled in firmware by redefining the TEMP_2_PIN
in file.

"pins_BTT_SKR_E3_TURBO.h"

 in directory 
 "../Marlin/Marlin/src/pins/lpc1769/"

 Comment out orginal provided by BTT, redefine with that name which is used in Marlin. Refere to config file for more details.

``` 
// #define TEMP_2_PIN       P1_30  // Onboard thermistor
#define TEMP_CHAMBER_PIN    P1_30
```





