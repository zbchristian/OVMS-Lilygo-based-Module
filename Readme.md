Open-Vehicle-Monitoring-System: Module based on Lilygo T-Call A7670E V1.0
=========================================================================
The Lilygo T-Call modules provide most of the hardware required for an OVMS module:
- ESP32-WROVER E with 4MB Flash and 8MB PSRAM
- Simcom A7670E 4G modem with GPS 
- Included antennas for 4G and GPS
- USB-C port for programming and powering during testing
- ext 5V powering possible via pin
- Price is around 35€ + tax

OVMS Requirements
----------------- 
- Connection to OBD-II connector
- Powering via 12V of the OBD-II connector
- OBD-II to DSub-9 cable 

Communication with the car via CAN-bus requires
- Physical layer(s) and CAN controller
- 12V to 5V DC/DC converter and including filtering

Hardware
========
The Lilygo board is placed onto a motherboard, either into a socket, or the pins can be directly soldered to the board. The motherboard provides the voltage for the Lilygo board and the DSub-9 connector to connect to the OBD-II.

The image shows the module, the antennas and the 3D printed case (Freecad and Step files availybe in the `CAD` folder). The motherboard slides into slots and the lid will hold it in place.
The lid allows to store the GPS antenna, but allows to add SMA connectors for external antennas as well. The 4G antenna can be stored in a 1mm high compartment inside the case.

![OVMS module based on Lilygo T-Call](/images/ovms-liulygo-module_w_case_500px.jpg)

*The Lilygo based module and case. The GPS-antenna is placed into a corresponding slot in the lid of the case*

The chosen orientation (Simcom 7670 on the top) of the Lilygo board allows for easy access to the antenna connectors. The ESP32 WROVER E is in this case located on the bottom side.  

Features of the motherboard
---------------------------
The Lilygo T-Call is placed on a motherboard 
- size 49 x 98 mm²
- DSub-9 connector (pinout compatible standard OVMS module) to connect to the OBD-II
- 12V car battery input from the OBD-II, or a separate power souce 
- Fuse and filtering (common mode choke and capacitors) of the input voltage 
- DC/DC converter to provide the Lilygo borad with 5V
    - Auto shutdown of the DC/DC converter at a voltage below 11.7V to prevent the drainage of the battery
    - A voltage of more than 11.9V is required to switch on again
- Two CAN busses are implemented
    - ESP32 internal requires only a physical layer
    - Second bus with an MCP2515 controller
    - Solder jumpers to enable 120 &Omega; bus termination resistors 
- Voltage divider to measure the battery voltage
- The schematic and board layout is provided for Autodesk Eagle
- The Gerber and bill of material is provided for the board production and assembly at JLCPCB 
- Nearly all components are placed on the bottom side of the PCB. This allows for an easy access, even when the Lilygo board is soldered to the motherboard
- 3D printable case to house the motherboard as well as the antennas
    - includes space to utilize external antennas via SMA connectors

![OVMS module based on Lilygo T-Call - Top side](/images/ovms-lilygo-module_top_500px.jpg)
![OVMS module based on Lilygo T-Call - Bottom side](/images/ovms-lilygo-module_bottom_500px.jpg)

*Top and bottom side of the module. Only a single CAN bus is assembled, because the module is used for a car with a single bus. Therefore the second physical layer and the MCP2515 is left empty*

Limitations
-----------
- Only 4MB Flash does no allow Over-the-Air updates of the firmware. So, each change requires access via the USB-C port 
- Only two CAN busses limits the application to certain cars
- No GPIO extender
- No SD-Card support

Firmware
--------
Except for the above mentioned limitation, the module is compatible with the standard OVMS firmware, BUT the pinout requires a remapping of the ESP32 pins.
This is implemented in the [forked OVMS repository](https://github.com/zbchristian/Open-Vehicle-Monitoring-System-3-Lilygo-Support) in the branch `Lilygo-support`. 
- Follow the standard OVMS documentation to clone the above repo, install additional packages and to install the required Espressif IDF
- Switch to branch `lilygo-support`
- In the folder `./vehicle/OVMS.V3` copy `support/sdkconfig.default.lilygo.hw10` to `sdkconfig` and `support/partition4M.csv` to `partition4M.csv`
- Follow the standard documentation to compile and flash the firmware via USB

Enjoy!