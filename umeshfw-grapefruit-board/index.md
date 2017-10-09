![grapefruit](img/grapefruit_small.jpg)

Work in progress.

uMeshFw Grapefruit board / debug platform
==============================================


During the last two years of sparse uMeshFw development I noticed that often a problem or a feature
cannot be properly simulated and tested. Most of the development was done on a qNode4 platform,
which (among the others) lacks support for many required peripherals and is not very debug-friendly.
It was (and still is) meant to be used in "production".

Therefore I decided to specify a new debug board / platform which should ease the development
of the uMeshFw firmware and uMesh protocol stack features.

Also, I had a feeling that qNodeX names are a bit boring.


Project status
--------------------

TODO




What did I miss (a list of components on the Grapefruit debug board)
----------------------------------------------------------------------

TODO: board schematic


### GPS receiver

A GPS receiver is present on the board to ease development of applications requiring precise
timing information or known position, such as routing and MAC protocols. A Quectel L70B model
was choosen because of its good local availability for a reasonable price (~8€) and its
features: 10Hz output, small size, 1PPS signal output and a very low current consumption
during tracking in "static conditions" (they claim 1.4mA only).

Its output is connected to a MCU USART interface, 1PPS output is connected to a MCU timer input
capture input and an additional LED is connected to a MCU GPIO output. It can be switched off
with a power switch using a P-MOSFET controllable by a MCU GPIO output.


### Power supply scheme

There are multiple possibilities how can be a mesh network node powered. The board tries to
cover all of them - the whole power supply chain can be reconfigured with jumpers for many
different power sources:

  * single LiPo/Li-Ion cell with onboard charger (3-4.5V). The charger can be powered from
    one of the USB ports or from the DC_IN connector. This setup is well suited for all
    mobile devices
  * single lithium or LiSOCl2 primary cell (3-3.6V) The charger can be disconnected for this
    battery type. This can be used to test low power sensor nodes supplied with a single
    CR2032 cell or a LiSOCl2 cell (AA, 1/2AA, etc.)
  * single alkaline cell (0.5-2V). 1.5V alkline cells are a feasible and cheap alternative
    if a user replaceable primary batteries are required
  * the board can be directly powered without a battery using one of the USB ports. This
    can be helpful during debugging when the board is attached to a host computer
  * DC_IN port can be used to directly power the board too. It can be used when the board
    is powered by an off-grid solar system (12 and 24V)


### Radio interfaces

The board offers two different footprints which covers nearly all available modules from
HopeRF. The list of compatible radio modules includes:

  * footprint 1: RFM69W and RFM69HW because of their good availability and low cost
  * footprint 2: RFM22B, RFM23B, RFM26W, RFM66W, RFM69HCW, RFM95W/96W/97W/98W


### Audio interfaces

For the purpose of testing possible uMesh applications in the field of voice transmission
and communication, a full duplex audio codec is provided on the board. It offers multiple
functions:

  * TRRS 3.5mm stereo headset jack connector for audio input and output with an automatic
    headset type detection
  * on-board mono electret microphone
  * connector for an external 4 or 8 ohm 1W speaker
  * up to 24 bit audio quality at 48kHz



### LCD display and buttons


### Microcontroller


### MicroSD socket




Documentation, source code and other resources
---------------------------------------------------

The latest version of the PDF manual is available [here](doc/grapefruit_board_manual.pdf).
It describes all you need to know to start working with the board. You need to have at
least a basic knowledge of programming and cross-compiling for STM32 microcontrollers.

Support for this board for the uMeshFw firmware and uBLoad bootloader can be found in the
corresponding repositories.

Everything can be found on the [Github project page](https://github.com/iqyx/grapefruit-board) too.



Contributing
----------------

TODO

----

https://commons.wikimedia.org/wiki/File:Citrus_paradisi_%28Grapefruit,_pink%29_white_bg.jpg

