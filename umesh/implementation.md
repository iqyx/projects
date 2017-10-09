uMesh stack implementation details
========================================================================

This document describes internals of uMesh network stack implementation
inside the uMesh secure node firmware. Only network related parts are
documented here. For other parts (RTOS, configuration interface, boot
process and bootloader, firmware update) see [umeshfw](umeshfw.md).

The whole network stack is divided into multiple independent modules
which have very strict responsibilities. All modules are hardware and
RTOS independent to a very high degree. Exceptions are device drivers
(which are using HAL provided by the RTOS) and thread management and
synchronization.

  - Radio drivers implement all required low-level communication with
    radio transceiver chips and modules. Generally RTOS-provided HAL
    is used to communicate over SPI or similar interface. Each driver
    also implements common driver interface which allows the stack to
    manage all connected radios.
  - Interface abstraction layer provides a container for all connected
    drivers of different types
  - Channel access method module polls layer 2 module for new
    buffers prepared for transmission and manages access to radio
    channel. This module can choose which data to transmit (and when)
    to enable multiple devices in the same broadcast domain to
    communicate with as low medium contention as possible.
  - Layer 2 module and  protocol manages L2 node identifiers (TID -
    temporary identifiers) and communication between directly adjacent
    nodes (neighbors) including error checking, data encryption and
    authentication. Information about neighbors are kept in a
    neighbor table inside this module. Layer 2 doesn't provide multihop
    communication nor any mechanisms to fill neighbor table. Without
    neighbors in the neighbor table, only unencrypted and unauthenticated
    communication can be made and only broadcast transmissions are
    possible.
  - Neighbor discovery periodically broadcast information about itself
    and waits for the same broadcasts from nodes in radio range.
    It then adds all neighbors to layer 2 neighbor table and tries to
    exchange keys with them and authenticate them. This allows layer 2
    module to communicate with neighbors securely.



Radio drivers
========================================================================




Interface abstraction layer
========================================================================


Channel/medium access methods
========================================================================


Layer 2 protocol
========================================================================


Neighbor discovery (layer 3 protocol)
========================================================================

Label-routed data protocol (layer 3 protocol)
========================================================================

uMesh on-demand routing protocol (uORP, layer 3 protocol)
========================================================================

L2 key exchange protocol (l2ke, layer 3 protcol)
========================================================================


