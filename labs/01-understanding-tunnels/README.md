# Lab 01 — Understanding Network Tunneling

## Difficulty

Beginner

## Objective

Understand the difference between an IP address, a port, a network connection and a tunnel.

## Scenario

You have two machines:

- **Kali** — test machine
- **Server** — laboratory machine

The Server is running a TCP service.

Your task is to connect to this service and understand how the connection works.

## Topology

```text
Kali
  |
  | TCP connection
  |
  v
Server
  |
  +-- TCP service :8080Tasks
Task 1 — Find the Server IP

On the Server run:

ip addr

Record the IP address.

Task 2 — Check the service

From Kali:

nc -vz <SERVER_IP> 8080

Task 3 — Inspect the connection

On Kali:

ss -tn

Look for the connection to port 8080.

Task 4 — Answer
What is an IP address?
What is a port?
What does TCP do?
What does LISTEN mean?
What does ESTABLISHED mean?
What would happen if the direct connection was blocked?
How could a tunnel solve this problem?
Success Criteria

You should be able to explain:

IP address
     +
Port
     +
Protocol
     =
Network connection

And explain why a tunnel can provide another path for network traffic.
