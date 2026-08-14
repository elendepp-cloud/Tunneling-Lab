# Lab 01 — Understanding Network Tunneling

## Difficulty

Beginner

## Objective

Understand the difference between an IP address, a port, a network connection and a tunnel.

## Scenario

You have two machines:

- Kali — test machine
- Server — laboratory machine

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
  +-- TCP service :8080
