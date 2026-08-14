# Lab 05 — Ligolo-ng

## Difficulty

Intermediate

## Objective

Understand how Ligolo-ng creates a tunnel between two machines
and provides a TUN interface for accessing an internal network.

## Scenario

You have two machines:

- Kali — attack/test machine
- Windows — pivot machine

The Windows machine has access to an internal network
that Kali cannot access directly.

Your task is to establish a Ligolo-ng tunnel and route
traffic through the Windows machine.

## Architecture

```
Kali
  |
  | Ligolo-ng Proxy
  |
  | TUN interface
  |
  |================ Tunnel ================|
                                             |
                                             v
                                      Ligolo-ng Agent
                                             |
                                             v
                                      Windows Network
                                             |
                                             v
                                      Internal Network
```
Key Components
Proxy

The Ligolo-ng Proxy runs on the machine controlling
the tunnel.

Agent

The Ligolo-ng Agent runs on the pivot machine.

TUN Interface

The TUN interface represents the virtual network interface
through which traffic is routed into the tunnel.

### Task 1 — Prepare the Lab Environment

Prepare two authorized laboratory machines:

- Kali Linux — Proxy
- Windows/Linux VM — Agent

Make sure both machines can communicate over the
laboratory network.

Record the IP addresses of both machines.

On Kali:

`ip addr`

On the Agent machine:

`ipconfig`

or:

`ip addr`

### Task 2 — Start the Ligolo-ng Proxy

On Kali, start the Ligolo-ng Proxy:

`./proxy`

The Proxy should start and listen for incoming Agent connections.

The Proxy is responsible for managing the tunnel.

### Task 3 — Start the Ligolo-ng Agent

On the Agent machine, start the Ligolo-ng Agent and connect it
to the authorized laboratory Proxy.

The Agent establishes the connection back to the Proxy.

The basic architecture is:

```
Kali
  |
  | Ligolo-ng Proxy
  |
  | Tunnel
  |
  v
Agent Machine
```

### Task 4 — Create the TUN Interface

On Kali, create or start the TUN interface used by Ligolo-ng.

Check the available network interfaces:

`ip addr`

Look for an interface created for the Ligolo-ng tunnel.

Then check the routing table:

`ip route`

The TUN interface provides a virtual network path
between Kali and the Ligolo-ng Agent.

The concept is:

Kali Application
       |
       v
   TUN Interface
       |
       v
   Ligolo-ng Proxy
       |
       | Tunnel
       |
       v
   Ligolo-ng Agent
       |
       v
 Internal Network

 ### Task 5 — Add a Route Through the Tunnel

First, identify the internal network that is reachable
through the Agent.

For example:

`10.10.10.0/24`

Add a route to that network through the Ligolo-ng TUN interface.

On Kali:

```
sudo ip route add 10.10.10.0/24 dev ligolo
```

Check the routing table:

`ip route`

You should see a route similar to:

`10.10.10.0/24 dev ligolo`

The traffic path is now:

Kali
  |
  | Route: 10.10.10.0/24
  v
TUN Interface
  |
  v
Ligolo-ng Proxy
  |
  | Tunnel
  v
Ligolo-ng Agent
  |
  v
10.10.10.0/24

### Task 6 — Test Connectivity Through the Tunnel

Choose an authorized host inside the internal network.

For example:

`10.10.10.10`

First, test basic connectivity:

```
ping -c 3 10.10.10.10
```

Then check whether an authorized TCP service is reachable:

```
nc -vz 10.10.10.10 80
```

If the route and tunnel are configured correctly,
the traffic should travel through the Ligolo-ng tunnel.

The traffic path is:

Kali
  |
  v
TUN Interface
  |
  v
Ligolo-ng Proxy
  |
  | Tunnel
  v
Ligolo-ng Agent
  |
  v
Internal Host
  |
  +-- TCP Service :80

## Questions

Answer the following questions:

1. What is the difference between the Ligolo-ng Proxy and Agent?
2. What is a TUN interface?
3. Why does Ligolo-ng use a TUN interface?
4. What happens when a route to the internal network is added?
5. Why can Kali access a network that was previously unreachable?
6. What is the role of the Agent machine?
7. What happens if the Ligolo-ng tunnel is disconnected?
8. How is Ligolo-ng different from SSH `-L` forwarding?
9. How is Ligolo-ng different from an SSH SOCKS proxy?
10. What is pivoting?

## Success Criteria

You should be able to explain:

`Kali → TUN → Ligolo-ng Proxy → Tunnel → Agent → Internal Network`

You should understand that Ligolo-ng creates a virtual
network path that allows traffic to be routed through
a pivot machine.

You should also be able to explain the relationship between:

`TUN interface`

`Routing table`

`Ligolo-ng tunnel`

`Agent`

`Internal network`
