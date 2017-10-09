
Projects
--------------

  * ![wip](../media/wip.png) [uMesh secure mesh network protocol stack](umesh) - 
    uMesh is a set of experimental communication protocols intended mainly for
    low data rate wireless mesh networks such as sensor networks for environmental
    data gathering, asset and people tracking, short message delivery and paging,
    narrowband voice communication etc. in scenarios, where common network
    infrastructure is not reachable and/or is not feasible for a particular purpose.
    Protocols are designed with high level of security in mind which could make
    them suitable for many critical and disaster recovery applications in the future.

  * ![wip](../media/wip.png) [uMesh node firmware](umeshfw) -
    uMesh node firmware combines many different projects and modules together with
    uMesh network protocol stack to create a solid platform for building wireless
    mesh nodes using various kinds of hardware and architectures.

  * [uMeshFw Bootloader (uBLoad)](ubload) -
    A (slightly more) secure bootloader for (not only) uMeshFw compatible wireless
    nodes

  * [qdlLibrary](qdlLibrary) -
    A simple display list rendering library. Main difference from other libraries
    for drawing objects on a canvas is that here the objects are rendered when
    display refresh occurs. Objects to be drawn are sorted as a nested boxes
    (hierarchical structure, tree). This approach uses much less memory and allows
    drawing on large displays without controller with dedicated display memory.
    It also makes "redrawing" screen and user interface widgets easier - there is
    no need to actively redraw anything after changes.

  * [SimpleFlashFileSystem](sffs) - 
    Simple filesystem originally developed for saving configuration and firmware
    update images on NOR flash memories in embedded systems. It is based on
    other similar filesystems (like yaffs, jffs), but it is made even simpler.
    No filenames or directory structure is supported, files are referenced by
    their IDs (like on smart cards). Filesystem is optimized for simplicity
    and is probably unoptimal in every single way.

  * [qNode5 - uMesh debug platform](qnode5) -
    uMeshFw compatible debug platform with STM32F407 MCU, radio, gps, LiPo accu and
    large STN transflective LCD display.

  * [qNode4 - uMesh-compatible solar powered low-cost node](qnode4) -
    First working hardware architecture capable of running uMeshFw. Contains low-cost
    STM32F401 Cortex-M4 MCU, RFM69 radio, battery step-down converter and solar
    step-up converter (without MPPT) feeding a LiPo charger.

  * [RF amplifier for 433MHz band](rfamp) - Amplifier and filter for amateur balloon
    telemetry reception

  * [Solar power management module](solar-pmm) - Power management module for solar charging 
    of small capacity LiPo cells controllable over I2C bus. Primarily intended to be used
    on uMeshFw solar-powered nodes.

  * ![wip](../media/wip.png) [LED lamp in a jar](jar-led-lamp)

  * ![wip](../media/wip.png) [GPS tracker with a simple LCD interface](lcd-gps-tracker)
    After much negative experience with Android GPS tracking (battery usage, bad
    reception) I decided to build my own dedicated GPS tracker despite the fact that
    multiple similar devices are readily available on the market.

  * ![wip](../media/wip.png) [uMeshFw Grapefruit debug board](umeshfw-grapefruit-board)
    During the last two years of sparse uMeshFw development I noticed that often a problem
    or a feature cannot be properly simulated and tested. Most of the development was done
    on a qNode4 platform, which (among the others) lacks support for many required
    peripherals and it is not very debug-friendly. It was (and still is) meant to be used
    in "production".
    Therefore I decided to specify a new debug board/platform which should ease the
    development of the uMeshFw firmware and uMesh protocol stack features.


Still undocumented
----------------------

  * [Simple http server (qhttpd)](qhttpd) -
    A simple HTTP server for embedded environments coded in plain C. Pages are saved in
    a CPIO archive converted to a initialized C array structure in compile time.
    Basic CGI functionality (C handlers are called) and HTTP authentication are both supported.
    Used in multiple different projects.

  * [TreeCli - a hierarchical, tree structured command line parser/interface](treecli) - 
    Treecli provides a convenient way to configure various embedded devices using
    common serial and network interfaces. It is inspired by command line interfaces
    commonly found in network gear (switches, routers, firewalls..).

  * [LineEdit library](lineedit) -
    Lineedit aims to be a lightweight command line editor usable over common stream
    interfaces (like sockets, serial ports, etc.). It was written to be used on an
    embedded platform and its features and code are optimized for embedded environments.

  * [hfsdr - HF SDR receiver](hfsdr) -
    HF software defined radio receiver inspired by other similar designs from
    amateur radio operators.



Planned
-------------

  * GPS-synchronized VLF receiver for the purpose of lightning detection and mapping.
    Project was started long time ago as a possible contribution to blitzortung.org,
    however it was abandoned later. There are plans to revive it with many improvements
    compared to the original design.

  * DC electrical grid (48V and 200V) for small communities with modules for solar MPPT
    conversion, backup battery charging, AC inverters, DC/DC converters for lighting, etc.

  * qPlc - A set of simple input and output modules controlled over a RS485 bus by a
    central controller (STM32/freertos or AR9331/linux) to provide PLC-like functionality
    for experimental home setups.

  * Bioethanol water heater - a small, electrically regulated bioethanol burner for water
    heating.


Tutorials
--------------

  * [433MHz ground plane antenna](433mhz_ground_plane_antenna)
  * [OV2640 Camera module on STM32 (without DCMI interface)](ov2640_stm32)



Conceptual
--------------

  * [Semantic Versioning for hardware components](hardware-semantic-versioning) -
    After some time using both the SemVer and GitFlow, I decided to try to apply similar
    rules to hardware development, mainly for system components and PCBs revision marking



Other
--------------

  * RFM69(CW) sub-1GHz wireless radio module range testing - part 1 still missing, [part 2](rfm69_range_testing_2), [part 3](rfm69_range_testing_3)
  * [Various resources regarding DC/DC converters](dcdc-converters)



