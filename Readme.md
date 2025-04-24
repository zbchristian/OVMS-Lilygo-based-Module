Open-Vehicle-Monitoring-System: Modules based on Lilygo T-Call A7670E V1.0
==========================================================================
The Lilygo T-Call modules provide most of the hardware required for an OVMS module:
- ESP32-WROVER E with 4MB Flash and 8MB PSRAM
- Simcom A7670E 4G modem with GPS 
- Included antennas for 4G and GPS
- USB-C port for programming and powering during testing
- 5V Powering 

OVMS Requirements
----------------- 
- Connection to OBD-II connector
- Powering via 12V of OBD-II
- OBD-II to sub-D9 cable 

Communication with the car via CAN-bus requires
- Physical layer(s) and CAN controller
- 12V to 5V DC/DC converter and including filtering

Features of the motherboard
---------------------------
The Lilygo T-Call is placed on a motherboard 
- size 49 x 98 mm²
- Sub-D9 connector (pinout compatible standard OVMS module)
- DC/DC converter to provide the Lilygo borad with 5V
    - Auto shutdown of converter at voltage below 11.7V to prevent the drainage of the battery
- Two CAN busses are implemented
    - ESP32 internal requires only a physical layer
    - Second bus with an MCP2515 controller
    - Solder jumpers to enable 120 &Omega; bus termination resistors 
- Voltage divider to measure the battery voltage (compatible with standard OVMS module)
- 3D printable case to house the motherboard as well as the antennas
    - includes space to utilize external antennas via SMA connectors

Limitations
-----------
- Only 4MB Flash does no allow Over-the-Air updates of the firmware.
- Only two CAN busses limits the application to certain cars

Firmware
--------
The standard version of the OVMS repository contains already support for the 4G Simcom module. 
Support for Lilygo Hardware has to be enabled before compiling the firmware.


Possible Modifications
----------------------
The height of the case can be reduced, in case, that the Lilygo is not placed into a socket, but soldered directly onto the motherboard.