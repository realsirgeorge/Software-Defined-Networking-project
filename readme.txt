Description:
This repository contains code for SDN controller applications developed as a final project for a university data communications and networking course. The project implements two SDN apps on top of the Floodlight controller:

Shortest Path Switching - Calculates shortest paths between hosts and installs rules to forward traffic along these paths. Includes extra credit features:
Flooding without loops using a minimum spanning tree
ECMP load balancing across equal cost paths based on TCP port
Load Balancer - Distributes new TCP connections across a pool of real servers in round robin order by rewriting packet headers.
The apps are developed in Java and tested using the Mininet network emulator. Documentation includes the project requirements, design documents, testing methodology, and final project report.



Part 3 - Layer 3 Routing Application

Design:

Create Host and Link objects to abstract topology information
Register for topology change events including link discovery and device events
When host added, compute all shortest paths to host using Bellman-Ford
Install rule per switch to match destination IP and forward out port towards next hop
Utilize packet proxies to send ARP replies on hosts' behalf
Recompute only affected shortest paths on topology changes to improve efficiency
Support ECMP with multiple next hops based on hash of fields
Implementation Details:

Stored hosts, switches and links in concurrent data structures
Parsed switch and host events to update respective data structures
Implemented Bellman-Ford algorithm to compute shortest paths
Generated routes by iterating over switches to install destination-based flows
Used additional match fields to divide traffic over multiple equal cost paths
Reacted to events by incrementally updating routing in affected parts of network
Added temporary prints in code for debugging: path changes, rule installations
Handled corner cases like hosts moving while maintaining connectivity


Part 4 - Load Balancer Application

Design:

Default rules to catch new connections to virtual IP and direct to controller
Controller selects backend server and installs connection-specific rules
Connection rules rewrite addresses and point to next table for routing
Match packets in pipeline stage 1 (LB app table)
Connection tracking with timeouts to expire old flows
Implementation Details:

Parse virtual IP config and create load balancer object models
Analyze packet headers in packet-in messages
Check TCP flags and lookup connection tracker
Assign each new connection Conversation object linked to backend server
Insert address rewriting flows in table with higher priority than defaults
Use action to point rewritten packets to routing app table
Created packet out messages to proxy ARP and TCP resets
Store connections in map structure with timers to remove old entries
Added prints for load distribution debugging




@realsirgeorge