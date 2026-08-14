# Lab 07 — Troubleshooting Tunnels

## Difficulty

Intermediate

## Objective

Learn how to diagnose common problems with network tunnels.

The goal is to understand how to determine whether a problem
is related to the service, SSH connection, tunnel, TUN interface,
routing table, or destination network.

## Scenario

A tunnel was configured in an authorized laboratory environment,
but the expected internal service cannot be reached.

Your task is to systematically identify where the connection fails.

## Troubleshooting Model

```
Application
    |
    v
Destination Port
    |
    v
Routing Table
    |
    v
TUN / Network Interface
    |
    v
Tunnel
    |
    v
Pivot / SSH Server
    |
    v
Internal Network
```

Key Principle

Do not immediately change random settings.

Troubleshoot the connection layer by layer.

##Tasks

### Task 1 — Check Network Interfaces

On Kali, list the available network interfaces:

`ip addr`

Identify:

- The main network interface
- The VPN/TUN interface, if one exists
- The Ligolo-ng TUN interface, if one exists

Record the interface names and IP addresses.

### Task 2 — Check the Routing Table

Display the routing table:

`ip route`

Look for a route to the expected destination network.

Ask:

- Does a route to the destination exist?
- Which interface will Linux use?
- Is the route pointing to the correct tunnel interface?

A missing or incorrect route can prevent traffic
from reaching the destination.

### Task 3 — Check Listening Services

Check listening TCP sockets:

```
ss -lnt
```

Identify which local ports are listening.

For an SSH tunnel, verify that the expected local
or remote forwarding port is present.

For example:

`127.0.0.1:9000`

### Task 4 — Check the Tunnel Connection

Verify that the tunnel endpoint is reachable.

For an SSH tunnel, check the SSH connection:

```
ss -tn
```

Look for an established connection to the SSH Server.

For a Ligolo-ng tunnel, verify that the Agent is connected
to the Proxy.

Check the tunnel status and confirm that the expected
session is active.

Ask:

- Is the tunnel session established?
- Is the expected endpoint reachable?
- Is the tunnel still running?
- Is the correct machine acting as the Proxy?
- Is the correct machine acting as the Agent?

### Task 5 — Test the Destination

After checking the interface, route, and tunnel,
test the destination service.

For example:

```
nc -vz <INTERNAL_IP> <PORT>
```

If the connection fails, determine which layer is responsible:

```
Interface
   ↓
Route
   ↓
Tunnel
   ↓
Destination
```

### Task 6 — Troubleshooting Checklist

Use the following checklist when a tunnel does not work:

1. Check the network interfaces:

`ip addr`

2. Check the routing table:

`ip route`

3. Check listening ports:

`ss -lnt`

4. Check active connections:

`ss -tn`

5. Verify that the tunnel session is established.

6. Verify that the destination host is reachable.

7. Verify that the destination service is listening.

Work through the checklist from top to bottom instead of
changing multiple settings at the same time.

## Questions

Answer the following questions:

1. What does `ip addr` show?
2. What does `ip route` show?
3. What is the difference between `ss -lnt` and `ss -tn`?
4. Why is a TUN interface not enough by itself?
5. Why is a routing table important for a tunnel?
6. How can you determine whether an SSH connection is established?
7. How can you determine whether a service is listening?
8. What could cause a tunnel to exist but traffic still fail?
9. Why should troubleshooting be performed layer by layer?
10. What is the first thing you would check if a tunnel stopped working?

## Success Criteria

You should be able to troubleshoot a tunnel using:

```
ip addr
```

```
ip route
```

```
ss -lnt
```

```
ss -tn
```

You should be able to determine whether a problem is related to:

`Network Interface`

`Routing`

`Listening Port`

`Tunnel`

`Destination Service`
