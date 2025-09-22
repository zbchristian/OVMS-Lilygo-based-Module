Open-Vehicle-Monitoring-System: Module based on Lilygo T-Call A7670E V1.0/V1.1
==============================================================================
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
The board revision B is suitable for the T-Call version 1.0 and 1.1 (different GPIO mapping).

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
- DC/DC converter to provide the Lilygo board with 5V
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
    - sufficient space in the lid to add SMA connectors for external antennas

![OVMS module based on Lilygo T-Call - Top side](/images/ovms-lilygo-module_top_500px.jpg)
![OVMS module based on Lilygo T-Call - Bottom side](/images/ovms-lilygo-module_bottom_500px.jpg)

*Top and bottom side of the module. Only a single CAN bus is assembled, because the module is used for a car with a single bus. Therefore the second physical layer and the MCP2515 is left empty*

Limitations
-----------
- The flash memory size of only 4MB Flash does no allow Over-the-Air updates of the firmware. So, each change requires access via the USB-C port 
- Only two CAN buses
- No GPIO extender
- No SD-Card support

Firmware
--------
Except for the above mentioned limitation, the module is compatible with the standard OVMS firmware, BUT the pinout requires a remapping of the ESP32 pins.
This is implemented in the [OVMS repository](https://github.com/openvehicles/Open-Vehicle-Monitoring-System-3). 
- Follow the standard OVMS documentation to clone the above repository, install the required additional packages and install the required Espressif IDF
- In the folder `./vehicle/OVMS.V3` copy `support/sdkconfig.lilygo_tc` to `sdkconfig` and `support/partition4M.csv` to `partition4M.csv`
- Run `make menuconfig` and go to `Component config -> Opene Vehicle Monitoring System (OVMS) -> Hardware support`.
  - Hardware Version should be 3.1
  - Enable GPIO remapping under `Choose GPIO Mapping`
  - Now a new menu entry appears in `Hardware support` called `File with GPIO mapping`
	- For the above `sdkconfig`, the default file is `lilygo_tc_v11.h`. This works for the Rev B motherboard and Lilygo T-Call V1.1. The file is located in the folder `components/gpio_maps`.
- Be sure, that the following OVMS components are disabled: MAX7317, external SWCAN, Over-The-Air update, SD card
- Disable the support for all cars except for the one you are interested in. This is required to meet the limited space in the Fash memory for the firmware
  - For the default `partitions4M.csv`, the available space for the firmware is `0x2f0000 = 2.94MB`
- Follow the standard documentation to compile and flash the firmware via USB
- Alternative GPIO mapping files are available and can be placed in the folder [`components/gpio_maps`](https://github.com/openvehicles/Open-Vehicle-Monitoring-System-3/tree/master/vehicle/OVMS.V3/components/gpio_maps). 
  A Readme is available describing the signal names and how to prepare a mapping file. Each mapping file (should) contains the information for which hardware its suitable.
- The above limitation for the firmware size can be lifted, by using a modified partition table (`partitions4M_full.csv`) from this repository
  - This reduces the `FAT` partition, the storage for the OVMS configuration, from `1MB` to `512kB`, which might cause problems in your setup!
  

Known Issues
------------
- An outdated firmware version of the Simcom A7670E can cause connection problems. The firmware can be updated according to the
[Modem Firmware Update Guide](https://github.com/Xinyuan-LilyGO/LilyGO-T-A76XX/blob/main/docs/update_fw.md). 
This requires to solder an USB cable to the corresponding pins on the Lilygo board (see [here](https://github.com/Xinyuan-LilyGO/LilyGO-T-A76XX/issues/180)).

Enjoy!