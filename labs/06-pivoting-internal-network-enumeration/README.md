# Lab 06 — Pivoting & Internal Network Enumeration

## Difficulty

Intermediate

## Objective

Understand the concept of network pivoting and how a compromised
or authorized pivot machine can provide access to an internal network.

The goal is to understand how routing and tunneling allow a test
machine to communicate with systems that are not directly reachable.

## Scenario

You have three network segments:

- Kali — test machine
- Pivot — machine with access to the internal network
- Internal Network — isolated laboratory network

Kali cannot directly reach the internal network.

The Pivot machine can reach it.

Your task is to route authorized traffic through the Pivot
and enumerate the internal laboratory network.

## Topology

```
                Direct access
Kali --------------------X----------------> Internal Network

Kali
  |
  | Tunnel
  v
Pivot Machine
  |
  | Internal interface
  v
Internal Network
  |
  +-- Host A
  +-- Host B
  +-- Host C

```

Key Concept

Pivoting means using an intermediate machine to reach
a network or system that is not directly accessible
from the original machine.


Tasks


### Task 1 — Identify the Pivot and Internal Network

Identify the network interfaces on the Pivot machine.

On Linux:

`ip addr`

On Windows:

`ipconfig`

Record the interfaces and their IP addresses.

The Pivot machine should have access to the internal
laboratory network.

Then identify the internal subnet.

For example:

`10.10.10.0/24`

The subnet used in this lab is only an example.
Use the actual subnet of your authorized laboratory environment.

### Task 2 — Verify the Route

On Kali, check the current routing table:

`ip route`

Identify whether the internal subnet is reachable directly.

If there is no route to the internal network, traffic
will not know where to go.

The goal of pivoting is to create a path:

Kali → Pivot → Internal Network

### Task 3 — Route Traffic Through the Pivot

Use the tunnel created in the previous lab.

First, identify the TUN interface:

`ip addr`

Then check the routing table:

`ip route`

Add a route for the internal laboratory subnet through
the tunnel interface.

Example:

`sudo ip route add 10.10.10.0/24 dev ligolo`

Verify the route:

`ip route`

The expected traffic path is:

Kali
  |
  | Route to internal subnet
  v
TUN Interface
  |
  v
Tunnel
  |
  v
Pivot
  |
  v
Internal Network

### Task 4 — Internal Network Discovery

Choose an authorized host range inside the laboratory network.

For example:

`10.10.10.0/24`

First, verify connectivity to a known host:

`ping -c 3 10.10.10.10`

Then perform a basic host discovery scan:

`nmap -sn 10.10.10.0/24`

The scan should be performed only against the
authorized laboratory network.

Record the hosts that respond.

The traffic path is:

Kali
  |
  v
TUN Interface
  |
  v
Ligolo-ng Tunnel
  |
  v
Pivot
  |
  v
Internal Network
  |
  +-- Host A
  +-- Host B
  +-- Host C


### Task 5 — Investigate an Internal Host

Choose one discovered host from the previous task.

Check which authorized TCP services are available:

```
nmap -sT -p 22,80,443 10.10.10.10
```
Record the open ports and identify the services.

Do not scan systems outside the authorized laboratory network.

## Questions

Answer the following questions:

1. What is network pivoting?
2. Why can Kali not directly access the internal network?
3. What is the role of the Pivot machine?
4. What does the TUN interface do?
5. Why is a route required before accessing the internal network?
6. What happens to traffic after it enters the tunnel?
7. Why can `nmap` discover hosts through the Pivot?
8. What is the difference between a Pivot and a normal destination host?
9. What happens if the tunnel is disconnected?
10. Why should internal network enumeration only be performed in an authorized environment?

## Success Criteria

You should be able to explain:

`Kali → TUN → Tunnel → Pivot → Internal Network`

You should understand that pivoting provides a network path
from one network segment to another through an intermediate machine.

You should also understand the relationship between:

`Pivot`

`TUN interface`

`Routing table`

`Tunnel`

`Internal Network`

`Network Enumeration`
