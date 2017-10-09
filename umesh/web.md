# Introduction

uMesh is a set of experimental communication protocols intended mainly
for low data rate wireless mesh networks such as sensor networks for
environmental data gathering, asset and people tracking, short message
delivery and paging, narrowband voice communication etc. They are applicable
in scenarios where common network infrastructure is not reachable and/or is not
feasible to be built.
Protocols are designed with high level of security in mind which could
make them suitable for many critical and disaster recovery applications
in the future. Many known algorithms and ideas from other networking
protocols are reused and repurposed for providing better resource saving
and higher anonymity and security.


# Features

Open specification and implementation compatible with many different computing platforms
ranging from multi-thousand-$ professional ones to small linux boards and sensor nodes
which cost less than $10. Features of the Umesh network stack can be adapted to the
amount of resources available while still maintaining compatibility with the others.

A hybrid (proactive/reactive) routing protocol with ideas from swarm intelligence
(ant colony optimization) is used. It uses virtual ants to discover routes, maintain
them, optimize them and carry data. In small network segments and if enough memory is
available, a proactive approach is used and the ants make pheromone trails which
help with destination finding. For large scale searches, the search is performed
reactively (on-demand) and only in specified network segments. When a path through the
network is built, the data is routed along the strongest pheromone trails, which can
change over time. This allows the route to be optimized and also provides many alternative
routes (although not that optimal). The protocol also supports segmentation into networks
(universes) and network segments (forrests), which improves network scalability significantly.

MAC layer with a dynamic and distributed time-slot and frequency allocation opposed to many
other wireless networks. This makes the network less dynamic for bursty data, but improves
determinism and lowers the jitter. This also enables network nodes to coexist with other
radio devices in the same band.
A simple CSMA MAC can be used too (eg. standard ethernet) or the data can be tunnelled over
TCP/IP to connect networks over the Internet.



# Motivation

There is a need to have a network stack with an open specification and implementation
usable on many computing platforms ranging from multi-thousand-$ professional ones to
small linux boards and sensor networks which cost less than $10.

It should be a matter of few minutes to set-up a new network node and connect it
to the rest of the network.

It should allow us to route our data through our infrastructure, which could span
many kilometers squared.


The protocol must have the ability to adapt to many platforms with different
amount of memory, computational and energy resources available.

Many networks use simple AODV-like protocols which have serious scalability issues.

# IoT disclaimer

In the recent years there is a big expansion of the IoT market. Much of buzz is
happening despite the fact that IoT is just a new name for an old concept.
Everyone wants to build networks for IoT devices, but uMesh is not meant to be
used for this purpose and it is not suitable for it (yet).

