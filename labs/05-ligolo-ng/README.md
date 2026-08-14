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
