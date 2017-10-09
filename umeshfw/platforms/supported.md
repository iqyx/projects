List of uMeshFw supported hardware platforms
====================================================


qNode4
-----------------

See [qNode4 project page](/embedded/qnode4) for detailed description. This is the first platform ever which can run uMeshFw. 5 nodes have been used to test first uMeshFw prototypes.



qNode5
----------------

See [qNode5 project page](/embedded/qnode5) for detailed description. This is considered to be a debug platform for uMeshFw as it contains large LCD, GPS receiver, microSD card slot and USB connector. It was used during testing to log radio coverage and show basic information about current network status.



qNode7 (work in progress)
----------------------------

Low-cost small platform, currently WIP.

  * STM32F401 microcontroller (64K SRAM & 256K flash by default)
  * SPI NOR flash (8mbit by default)
  * RFM24 transceiver (si446x chip)
  * single AA cell power source with step-up regulator to 1.8V
  * SMA connector for antenna
  * 1.8V level USART for serial console



List of obsolete and unsupported hardware platforms
=====================================================



qNode1
------------------

Idea of this hardware platform was to use extremely cheap STM32F050 MCU with STM Spirit1
radio transceiver. Power source should have been only one CR2032 lithium battery. Prototype
was 50% drawn, but never manufactured or tested. There is a possibility that similar low cost
platform will be made again.



qNode2
------------------

STM32L151 MCU with Spirit1 radio transceiver. Board also contains LiPo charging circuit
using SPV1040 solar charger with MPPT. Boards have been manufactured but never tested.




qNode3
-----------------

Dual radio with RFM69(H)W and RFM69CW compatible footprints. STM32F401 MCU, solar step-up
converter, LiPo charger, LiPo step-down regulator. Board have unfinished routing and
have never been built.

Board with similar functionality is planned - STM32F401 MCU + two slots for radio modules.

