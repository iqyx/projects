



uMesh network protocol stack
=====================================




Abstract
-----------------------------


**uMesh is a set of experimental communication protocols intended mainly
for low data rate wireless mesh networks such as sensor networks for
environmental data gathering, asset and people tracking, short message
delivery and paging, narrowband voice communication etc. in scenarios,
where common network infrastructure is not reachable and/or is not
feasible for a particular purpose.
Protocols are designed with high level of security in mind which could
make them suitable for many critical and disaster recovery applications
in the future. Many known algorithms and ideas from other networking
protocols are reused and repurposed for providing better resource saving
and higher anonymity and security.**




Main features
-------------------

  * on-demand route discovery based on AODV protocol - routes are computed
    and discovered by controlled flooding when needed. This allows the network
    to scale on resource constrained devices while introducing only small
    delay on first packet delivery. Result is a network with no centralized
    knowledge of topology.

  * label (tag) based routing known from MPLS and similar protocols. Routing
    labels are allocated on hop-by-hop basis during path establishment phase.

  * constraint-based (or limit-based) route discovery - nodes are allowed
    to specify which constraints for various link/path metrics have to be
    met for new paths (eg. minimum bandwidth required, maximum allowable
    latency, etc.). Intermediate nodes can then allocate dedicated resources
    for discovered paths such as buffers or radio time slots. Alternative
    paths can be discovered for failover and load balancing. This concept
    is not new and is used in some existing network protocols to implement QoS.

  * nodes are continuously advertising themselves with “hello” packets
    (beacons) to let their neighbor know they are alive. Hello packets contain
    ephemeral node identifiers only which provide some sort of node anonymity
    to unauthenticated eavesdropper.

  * neighbours are required to authenticate to each other to prevent
    man-in-the-middle attacks and discovering or spoofing routes by a third
    party. Authentication is performed by ECDSA in an ECDH secured channel
    to prevent revealing node signatures.

  * new nodes are allowed to join the network only if their public key is
    present in the surrounding nodes (non-PKI mode) or if they have a valid
    certificate signed by a CA with public key present in the surrounding
    nodes (PKI mode). This gives node owner the possibility to control with
    who can the node communicate and create adjacency.

  * network provides no global addressing scheme, addresses could be anything
    from IPv4/IPv6 to random byte strings. Nodes route packets forward based
    on temporary node identifiers and local routing labels. There's no way to
    find out the originating node of the packet without knowing its encrypted
    payload or owning all nodes along the path.

  * the stack can do unicast, neighbor broadcast, multicast and anycast
    routing which allow more advanced network services to be used.





Implemented protocols
------------------------------------

  * layer 2 protocol for communication between adjacent nodes providing
    addressing based on temporary identifiers, error checking (CRC-16),
    encryption and packet authentication (ChaCha20/Poly1305 or AES128/HMAC-SHA256),
    optimizations to reduce protocol overhead

  * neighbor discovery protocol for finding adjacent nodes and building neighbor
    table which provides information required for routing

  * layer 2 key exchange protocol tries to compute shared keys with all available
    neighbors (used to encrypt all layer 2 traffic) and authenticates them

  * layer 3 label-routed data protocol (LRDP) adds packet headers with routing
    labels and all required information needed for multi-hop packet delivery

  * AODV-based routing protocol for route discovery and propagation of other
    routing related information






References, used ideas and software
--------------------------------------

### AODV-based routing


### Used software

  * Curve25519 elliptic curve Diffie-Hellman, Code is based on public domain code
    curve25519-donna (Copyright 2008, Google Inc), Based on public domain sources
    and papers by Daniel J. Bernstein, Niels Duif, Tanja Lange, Peter Schwabe and
    Bo-Yin Yang. More information about curve25519 can be found on
    [http://cr.yp.to/ecdh.html](http://cr.yp.to/ecdh.html)

  * Ed25519-donna implementation, Based on public domain sources by Andrew M.
    <liquidsun@gmail.com> Based on public domain sources and papers by Daniel J. Bernstein,
    Niels Duif, Tanja Lange, Peter Schwabe and Bo-Yin Yang. More informatin on
    [http://ed25519.cr.yp.to](http://ed25519.cr.yp.to)

  * ChaCha20, Based on code by chacha-ref.c version 20080118, D. J. Bernstein, Public domain.

  * Poly1305, Based on code by 20080912, D. J. Bernstein, Public domain.

  * SHA-2, FIPS 180-2 SHA-224/256/384/512 implementation, Copyright (C) 2005, 2007 Olivier Gay
    <olivier.gay@a3.epfl.ch> All rights reserved.







Project status
----------------------

Jan/2014 - basic protocol ideas implemented and simulated

Ongoing changes:

- packet structure changes to reflect ISO OSI model to allow splitting into layers
- different physical layer transport support
- multiple interface support
- changes of route discovery protocol towards RFC3561 (Ad hoc On-Demand Distance Vector (AODV) Routing)

May/2014 - most of the uMesh codebase rewritten






Possible improvements / future plans
--------------------------------------

- there are planned changes to address EN 300 220 compliance for 433MHz and 868MHz ISM bands. Multiple constrains will have to be implemented including automatic power adjustment and transmit duty control
- implementing radio frequency agility could leverage some ETSI requirements and thus provide more usable bandwidth/duty. Multiple hopping/agility schemes are possible
- implementing some cognitive radio concepts could possibly provide better interference detection, resistance and interoperability with other devices in the same bands.






Source code
--------------------









No external library dependencies are required for uMesh stack. All stack code is maintained in uMesh respository.







Licensing
------------------

There was an uncertainty whether to release uMesh protocol stack under a permissive license or not.
The result is GNU GPLv3 (restrictive) with possible linking exception for stable releases.

Some of the main reasons are described below:

  * There is a need to develop usable and feasible implementation of mesh networking protocol
    stack with features and ideas forgotten somewhere in university papers, researches and laboratories.

  * There is a need to have publicly available and auditable implementation with at least a degree of
    security that is allowable on low rate networks on such constrained devices like uMesh is targetted for.

  * Used protocols and combinations of various algorithms are still considered experimental and development
    sources are not suited for many tasks. If anyone want to participate and improve it, they can
    contribute to official repositories and we encourage people to do so. Official stable versions
    can be used afterwards.

  * There is no need to develop another network stack for closed source applications and let companies
    hide and obfuscate things like they usually do. There are many alternatives targetted for such
    applications. Documentation and implementation availability for them is variable. Hiding and obfuscation
    doesn't provide any benefits neither for companies (they loose community support) nor the users.
    It just makes things harder or unable to accomplish. I want to make hiding harder. You shouldn't
    hide things that were made to be open. Therefore you are not allowed to modify uMesh without
    revealing your modifications and rest of your application (it becomes derived work if you link
    to uMesh sources). The only way to make a modification to uMesh is by contributing to offial repositories
    and waiting for stable release with linking exception.

  * There is a need to assure contributors that their public contributed code will not be just
    stolen, modified for different purpose and used in a product for making money.


If not stated otherwise, all original code is Copyright (C) 2014, Marek Koza, qyx@krtko.org

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.





Contributing
------------------------------------





