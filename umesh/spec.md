Abstract
========================================================================

uMesh is a set of experimental communication protocols intended mainly
for low data rate wireless mesh networks such as sensor networks for
environmental data gathering, asset and people tracking, short message
delivery and paging, narrowband voice communication etc. in scenarios,
where common network infrastructure is not reachable and/or is not
feasible for particular purpose.
Protocols are designed with high level of security in mind which could
make them suitable for many critical and disaster recovery applications
in the future.


Terminology and list of abbreviations
========================================================================



Layer 1 communication
========================================================================


Supported rates and modulations
--------------------------------------

Medium access methods
--------------------------------------




Layer 2 communication
========================================================================

Node identification
--------------------------------------

Communication on the 2nd layer can occur between adjacent nodes only.
Many legacy protocols use Unique Identifiers such as EUI-48 (48-bit
Extended Unique Identifier) to identify layer 2 network participants.
As a result, 12 additional bytes are transferred in each frame which
introduces significant overhead in low data rate networks. Such layer 2
identifiers are only required to uniquely distinguish network nodes in
the same broadcast domain and can be considerably shortened for this
purpose. However with the lack of central authority managing these
identifiers, identifier collision problem have to be addressed.

Every node uses TID (temporary identifier) for its identification. TID
is any randomly choosen unsigned number from 1 to 2^32 - 1. Every node
MUST generate its TID from random source at least on power-up. It SHOULD
generate new TID after configurable time has elapsed to maintain some
minimal degree of node anonymity. A node is not required to generate its
TID from the whole 32bit space, eg. new identifiers could be generated
from 0-255 range (risking higher probability of collisions).

A node accepts received packet only if one of the following criteria is met:

  - received packet has broadcast flag set
  - received packet "dtid" value is same as current receiving node TID
  - received packet "dtid" value is same asi previous value of TID,
    but only for a configurable amount of time after new TID becomes
    efficient

It means that if identifier collision occurs, the same data is received
by multiple nodes. However, this situation is prevented in practice:

  - if the two nodes have already finished layer 2 key exchange, colliding
    node cannot receive data because of key mismatch
  - a node cannot initiate key exchange with another node with the same
    TID, hence no communication will be done between colliding nodes
  - if a node want to initiate key exchange with another node which has
    TID colliding with a third node, key exchange will fail in first
    (Diffie-Hellman) phase as a result of multiple incompatible received
    answers



L2 packet structure
--------------------------------------

Each layer 2 packet is made of fields. Some fields are optional and their
inclusion in the packets depends on flags in the control field.

<pre>
Field		Size		Description
---------------------------------------------------------------
Length		1 byte		Length of the whole packet including
				the header and MAC/CRC
Control         1-n bytes	Packet control field
Dst TID (dtid)	1-5 bytes	Destination TID
Src TID (stid)	1-5 bytes	TID of the source
Data		n bytes         Plaintext or encrypted data payload
MAC		n bytes         Message authentication code (MAC) or CRC
</pre>


Length field is L1/radio layer dependent and is always 1 byte long. It
includes whole packet header and MAC/CRC. The minimum valid packet length
is 2 bytes - one control byte with broadcast flag set and one source TID
byte. Packet can contain no data.




Packet control field is encoded in a variable length manner to reduce
overhead of small packets with common payload and parameters. In every
byte of packet control field, only 7 least significant bits (LSb's) are
used to carry information, MSb is used to indicate if more bytes follow.
This MSb is called "ext" (extension) and is set to "1" if another byte
follows and to "0" if it is the last one.





Byte 1 of packet control field

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| Ext    | bcast  | Enc 1  | Enc 0  | L3 1   | L3 0   | Reserved        |
+--------+--------+--------+--------+--------+--------+--------+--------+

Ext - 		set to "1" if another byte follows
bcast -		if set to "1", packet is broadcasted to all nodes in the
		range, in this case, no dtid field is present in the packet
Enc1, Enc0 - 	2 bits of packet encryption and authentication mode
L3_1, L3_0 -	2 bits of L3 protocol
Reserved -	not used, SHOULD be set to 0
</pre>



Byte 2 of packet control field

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 15     | 14     | 13     | 12     | 11     | 10     | 9      | 8      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| Ext    | Enc 2  | Reserved                          | L3 3   | L3 2   |
+--------+--------+--------+--------+--------+--------+--------+--------+

Ext - 		set to "1" if another byte follows
Enc2 - 		1 bit of packet encryption and authentication mode
L3_3, L3_2 -	another 2 bits if L3 protocol
Reserved - 	not used, SHOULD be set to 0
</pre>


The most common encryption and packet authentication methods are listed
in the first 4 lines with Enc2 bit set to "0". This allows the packet
to specify basic encryption and authentication modes also in cases when
1 byte control field is used.

The same apply to layer 3 protocols - the most commonly used protocols
have assigned numbers from 0 to 3 to allow them to be specified with
1 byte control field.



Encryption and authentication modes
------------------------------------------------------------------------

There are basically 3 types of encryption supported:

  - no encryption and no error detection - used in occasions when no keys
    have been established yet or the other party doesn't support any
    encryption, packet authentication nor error checking. There SHOULD
    be a configuration option to disable reception of such frames
  - error detection only - used during the first phases of key exchange
    when no keys have been established yet. Can be optionally used for
    communication between nodes which don't support any form of leyer 2
    encryption. There SHOULD be a configuration option to disable
    reception of such frames except in key exchange protocol.
  - encryption and authentication - the whole communication is encrypted
    and authenticated using selected cipher/method.

In addition to methods provided by layer 2 protocol, another methods for
error detection and correction may be used directly in layer 1.


<pre>
Enc2-0 	|Type
------------------------------------------------------------------------
0 0 0 	| no encryption, no message authentication, no error checking
0 0 1 	| CRC-16 CCITT poly 0x8110, no enc, no mac
0 1 0 	| ChaCha20, Poly1305, 2 byte auth tag (truncated from 16 bytes)
0 1 1 	| AES-128, HMAC-SHA256, 2 byte auth tag (truncated from 32 bytes)
1 0 0 	| CRC-32, no enc, no mac
1 0 1 	| ChaCha20, Poly1305, 4 byte auth tag (truncated from 16 bytes)
1 1 0 	| AES-128, HMAC-SHA256, 4 byte auth tag (truncated from 32 bytes)
1 1 1 	| reserved 
</pre>

Note:	Enc2 bit is not always present in the control field. It is read
	as "0" in this case (eg. when control field is 1 byte only)

Note:	Enc3 bit can be added in the future to extend the list of
	available ciphers.





Destination and source TID
--------------------------------------

Destination TID ("dtid") of the packet is contained only if the "bcast"
bit in the control field is set to "0". In this case "dtid" field contain
TID of the receivind node.

Source TID MUST be contained in the packet.

Destination and source TID's are encoded in a manner similar to control
field encoding. Every byte can carry 7 bits of the number. Most significant
bit is set to "1" if another byte follows and to "0" otherwise (last byte).

If the TID is lower than 128, single byte can be used to encode it:

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| 0      | bit 6  | bit 5  | bit 4  | bit 3  | bit 2  | bit 1  | bit 0  |
+--------+--------+--------+--------+--------+--------+--------+--------+
</pre>

If it is lower than 16384, it can be encoded using 2 bytes:

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 15     | 14     | 13     | 12     | 11     | 10     | 9      | 8      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| 0      | bit 13 | bit 12 | bit 11 | bit 10 | bit 9  | bit 8  | bit 7  |
+--------+--------+--------+--------+--------+--------+--------+--------+

+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| 1      | bit 6  | bit 5  | bit 4  | bit 3  | bit 2  | bit 1  | bit 0  |
+--------+--------+--------+--------+--------+--------+--------+--------+
</pre>

Up to 5 bytes can be used to encode whole 32bit "dtid" or "stid".











========================================================================



ChaCha20 + Poly1305
--------------------------------------

Implementation of ChaCha20+Poly1305 encryption and MAC basically follows
IETF draft-agl-tls-chacha20poly1305-01 with some modifications. 32 byte
key and 8 byte nonce negotiated during key exchange process together with
two counter values is used to compute 64 byte ChaCha20 output. The first
4 byte counter is being incremented, the second is used only to distinguish
between keystream for MAC and encryption. When ChaCha20 is used to compute
key for Poly1305 message authentication, counter 2 is set to 0 and counter
1 is set to cipher block counter value which the packet starts with. Only
first 32 bytes of the 64 byte output of ChaCha20 is used as a key for
Poly1305. Message authentication tag is computed and optionally truncated
to required length depending on choosen mode. Tag is then appended to the
packet and packet length is modified accordingly. When used to encrypt
packet data in CTR mode, counter 2 is set to 1 and counter 1 is set to
cipher block counter value of the actual block for encryption/decryption.
64 byte block of packet data is then XOR'ed with generated 64 bytes of
keystream. If the packet size is smaller than the cipher block size
(64 bytes), remaining packet bytes are padded with keystream bytes before
XOR'ing. This results in encrypted data padded with zeroes which can be
truncated.


Using no error detection, encryption and MAC
--------------------------------------

Transmitting data without any of these mechanisms pose several risks:

  - packets can be received and forwarded corrupted
  - any capable radio transmitter can inject invalid packets to the
    network or intercept unencrypted traffic
  - replay attacks are possible

It is recommended to encrypt and authenticate all traffic between nodes
by default using fast ChaCha20 + Poly1305 scheme and to reject all packets
with no CRC, no encryption and no MAC.

Using truncated MAC
--------------------------------------

Low bitrate networks practically limit possible packet forgery even if
short MAC's are used as an attacker has very limited time and available
bandwidth to successfully change/inject packet before rekeying occurs.



Layer 3 communication
========================================================================







Layer 3 protocols
========================================================================


Label-routed data protocol
--------------------------------------


AODV-based routing protocol
--------------------------------------


uMesh network uses AODV-based protocol for route discovery and maintenance
called uMesh on-demand routing protocol (uORP). It is extended subset
of original AODV protocol defined in http://www.ietf.org/rfc/rfc3561.txt.
Original AODV features which are implemented are:

  - RREQ route request messages flooded through the network
  - unicast RREP route response messages carrying information about
    discovered routes
  - RRER messages informing adjacent nodes about broken routes
  - some features reducing broadcast storms (time-to-live of the messages,
    exponential backoff mechanism, circle search, etc)

However, there are many modifications described below.



Route discovery process
------------------------------------------------------------------------



Upon reception of a packet from higher layer with unknown destination,
route discovery process has to be started to find next hop of the packet.
AODV does this by controlled flooding of route request (RREQ) packets
containing information about the route we are willing to find, source
address, sequence numbers, path cost and time to live of the packet.
Every node checks its routing table if the requested route is known and
optionally responds with route response packet (RREP) back to the source
of the RREQ. However, this approach is not usable in uMesh stack as our
routing tables do not contain any node address information. Only the last
node (the one owning the requested address) has the ability to respond
with a route response packet (RREP) back to the previous hop.

Generally every source node needs at least one route to destination of
its data. However there is no theoretical limit of discovered routes
between two communicating nodes offering the advantage to gratuitously
discover more alternative routes while the original route is still alive.
Additionally every source node can attach class information to discovered
routes and use them later to classify and prioritize outgoing traffic.
uORP also uses specific fields in RREQ messages to implement constraint
based routing and to find different routes with appropriate parameters.
This mechanism allows implementing simple QoS (quality of service).


Fields used to specify requested route parameters are called constraints.
Every constraints have following items, some of them are optional:

  - constraint type - defines metric which is used in comparisons when
    evaluating incoming RREQ messages
  - constraint value - a value accumulated hop-by-hop from all nodes
    the RREQ message was forwarded through
  - constraint threshold (optional) - used with conjunction with "at most"
    and "at least" functions. It is not included in the RREQ message
    when "min" and "max" functions are specified.
  - constraint function - defines how new RREQ messages are evaluated and
    compared with constraint values saved in pending request table



RREQ (route request) messages
--------------------------------------

RREQ message is formed and filled with required information on requesting
node. For each route discovery process, new route request ID is used.
It is then broadcasted to all adjacent nodes. When the RREQ message is
received, node extract its contraints and evaluates them to find out if
the RREQ message can be forwarded further. Rules for forwarding RREQ
messages are following:

  - If RREQ with unknown (new) request ID is received, it is placed in a
    table of route discovery processes in progress and is always forwarded.
  - if request ID is found, all constraints with "at least" and "at most"
    functions are evaluated. If RREQ message fails at least one constraint,
    message is not forwarded further.
  - all constraints with functions "min" and "max" are compared to last
    known values saved in the pending route discovery table in order as
    they appear in the RREQ message. If a constraint compares as equal,
    next constraint is evaluated.
  - if RREQ message passed all test, it is flooded further to all adjacent
    nodes


Cumulated vs. non-cumulated constraints
------------------------------------------------------------------------



RREQ packet structure
------------------------------------------------------------------------



<pre>
Field		Size	Type		Description
------------------------------------------------------------------------
Header		n B	v_bitstring	RREQ message header
RREQ ID		n B	v_uinteger	route request unique identifier
address		n B	bytestring	address of the destination node
constraint hdr	1 B	bitstring	header of the first constraint
cumulated val	n B	v_uinteger	cumulated constraint value from
					previous nodes
threshold val	n B	v_uinteger	threshold value of the constraint,
					optional. Valid only for "at most"
					and "at least" functions.
</pre>

Byte 1 of RREQ header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | T2     | T1     | T0     | A3     | A2     | A1     | A0     |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise (common to all message types)
T2-T0 -	uORP packet type, MUST be set to "000" for RREQ messages
	(common to all message types)
A3-A0 -	requesting address length in bytes + 1 (least significant 4 bits),
	addresses can be 1 to 16 bytes long, additional header byte must
	be contained to specify addresses longer than 16 bytes
</pre>



Byte 2 of RREQ header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 15     | 14     | 13     | 12     | 11     | 10     | 9      | 8      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | A5     | A4     | Reserved                                   |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise (common to all message types)
Reserved - MUST be set to "0"
</pre>


Byte 1 of constraint header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | C3     | C2     | C1     | C0     | Res    | F1     | F0     |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise. Reserved, MUST be set to "0".
Res -	reserved, MUST be set to "0"
C3-C0 -	constraint type according to table below
F2-F0 -	constraint filter function according to table below
</pre>



Table of applicable constraints:
------------------------------------------------------------------------


<pre>
C3-C0	| Constraint description
------------------------------------------------------------------------
0 0 0 0	| Path hop count. Can be used with "minimum" and "at most"
	| functions. This constraint can be used to limit RREQ message
	| time-to-live. Units = "hops", minimum = 0 (localhost),
	| maximum = n
0 0 0 1	| Cumulated link cost computed as specified in "Computing link
	| cost" chapter. Units = none, minimum = 0, maximum = 127
0 0 1 0 | Available free bandwidth. Units = kBit/s
</pre>


Constraint functions:
------------------------------------------------------------------------

<pre>
F1-F0	| Function
------------------------------------------------------------------------
0 0	| Minimum. When two different routes are compared, select the one
	| with lower value. This ensures that a route with minimum value
	| of selected constraint will be discovered.
0 1	| Maximum. Effect is similar as "Minimum" above, a route with
	| maximum value of selected constraint will be discovered.
1 0	| Lower than or equal (At most). Route is checked against threshold
	| value included in the RREQ message. It passes if the cumulated
	| constraint value is lower than or equal to threshold.This
	| function can be used to find routes with latency below defined
	| threshold or routes with constrained hop counts (circular search).
1 1	| More than or equal (At least). The same as above, route request
	| passes the check if the actual cummulated value is bigger than
	| or equal to the value in constraint threshold. This function can
	| be used to find a route with free bandwidth of minimum defined
	| value.
</pre>

RREP (route response) messages
------------------------------------------------------------------------

Route response messages can be generated on the node owning the requested
identifier only. Because label routing is used, there is no possibility
for intermediate node to answer arrived RREQ messages with RREP message.

Reception of RREP message by intermediate node causes node to enter second
stage of route discovery - no further RREQ messages are being forwarded.
Instead, RREP message will be sent RREQ message originator.

Route response messages also carry information about allocated label from
originating node to node which originally requested the route.

After the header and mandatory fields, there is variable number of property
fields with format similar to RREQ message constraints. These properties
describe a newly found route.


RREP packet structure
------------------------------------------------------------------------


<pre>
Field		Size	Type		Description
------------------------------------------------------------------------
Header		n B	v_bitstring	RREP message header
RREQ ID		n B	v_uinteger	route request unique identifier
label		n B	v_uinteger	allocated label
property hdr	1 B	bitstring	header of the first route property
property val	n B	v_uinteger	property value
</pre>

Byte 1 of RREP header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | T2     | T1     | T0     | Res                               |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise (common to all message types)
T2-T0 -	uORP packet type, MUST be set to "001" for RREP messages
	(common to all message types)
Res -	reserved, MUST be set to "0"
</pre>

Byte 1 of property header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | C3     | C2     | C1     | C0     | Res                      |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise. Reserved, MUST be set to "0".
Res -	reserved, MUST be set to "0"
C3-C0 -	property type (same as constraint type)
</pre>




RERR (route error) messages
------------------------------------------------------------------------

Route error messages allow errors to propagate between nodes during
route discovery and route maintenance. A route error can occur:

  - when a link between nodes is broken
  - when a link between nodes is going to break (ie. there is very
    high probablity that the link will break in defined time). This error
    can be sent by a node which measures and computes its metric trends.
  - if any of the constraints defined in RREQ message fails

There is a list of optional RERR parameters appended to each RERR
message. It depends on actual error type which parameters will be included.



RERR packet structure
------------------------------------------------------------------------


<pre>
Field		Size	Type		Description
------------------------------------------------------------------------
Header		n B	v_bitstring	RERR message header
param hdr	1 B	bitstring	header of the first RERR parameter
param val	n B	v_uinteger	value of the first RERR parameter
</pre>

Byte 1 of RERR header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | T2     | T1     | T0     | E2     | E1     | E0     | Res    |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise (common to all message types)
T2-T0 -	uORP packet type, MUST be set to "010" for RERR messages
	(common to all message types)
E2-E0 -	error type
Res -	reserved, MUST be set to "0"
</pre>

Byte 1 of RERR parameter header

<pre>
+--------+--------+--------+--------+--------+--------+--------+--------+
| 7      | 6      | 5      | 4      | 3      | 2      | 1      | 0      |
+--------+--------+--------+--------+--------+--------+--------+--------+
| ext    | Res                      | C3     | C2     | C1     | C0     |
+--------+--------+--------+--------+--------+--------+--------+--------+

ext - 	header extension - set to "1" if another header byte follows,
	set to "0" otherwise. Reserved, MUST be set to "0".
Res -	reserved, MUST be set to "0"
C3-C0 -	parameter type
</pre>




Route error types
------------------------------------------------------------------------

<pre>
E2-E0	| Error type
------------------------------------------------------------------------
0 0 0	| A packet with invalid route label was received. This can happen
	| if a route was invalidated as a result of broken link to neighbor
	| node. One or multiple "label" parameters MUST be included.
0 0 1   | Link to directly adjacent node (neighbor) was broken. This error
	| is proactively sent to prevhops of all routes which had their
	| nexthop accessible through failed link. One or multiple "label"
	| parameters MUST be included.
0 1 0	| Required constraint cannot be met during 
</pre>


