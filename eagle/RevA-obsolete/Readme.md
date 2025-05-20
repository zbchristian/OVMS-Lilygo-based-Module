Obsolete Revision A of the motherboard
======================================
The first version of the motherboard is only suitable for the Lilygo T-Call A7670E V1.0!

Due to re-arranged GPIOs is the board not compatible with T-Call V1.1.

The revision B board can be used for both T-Call versions, but requires a different GPIO mapping in the software.

Modification of a Rev A board into a Rev B: 
- Cut connection of GPIO 32 to the transceiver chip (CAN-TX) -> connect with GPIO 2
- Cut the conenction of GPIO 33 (CAN transceiver RS) -> conenct with GPIO 21