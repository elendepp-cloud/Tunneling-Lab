# Lab 04 — SSH SOCKS Proxy

## Difficulty

Intermediate

## Objective

Understand how an SSH SOCKS proxy works.

The goal is to understand how a SOCKS proxy can route
network connections through an SSH server.

## Scenario

You have two machines:

- Kali — test machine
- SSH Server — laboratory machine

The SSH Server can reach a network that Kali cannot
directly access.

Your task is to create a SOCKS proxy through SSH
and use it to route network traffic.

## Topology

```
Kali
  |
  | SOCKS Proxy :1080
  |
  v
SSH Server
  |
  v
Internal Network
```

## Tasks

### Task 1 — Verify SSH Access

From Kali, connect to the SSH Server:

```
ssh <USER>@<SSH_SERVER_IP>
```

Verify that you can successfully authenticate to the SSH Server.

Keep the SSH session available for the next task.

### Task 2 — Create the SOCKS Proxy

From Kali, create a SOCKS proxy:

```
ssh -D 1080 <USER>@<SSH_SERVER_IP>
```

Keep the SSH session open.

The option `-D` creates a dynamic port forward.

The SOCKS proxy listens locally on:

```
127.0.0.1:1080
```

### Task 3 — Use the SOCKS Proxy

On Kali, configure a SOCKS-aware tool to use:

`127.0.0.1:1080`

For example, with `proxychains`:

```
proxychains curl http://<INTERNAL_IP>:8080
```

The connection should be routed through the SSH SOCKS proxy.

The traffic path is:

Kali
   |
   | SOCKS :1080
   v
SSH Tunnel
   |
   v
SSH Server
   |
   v
Internal Service :8080

### Task 4 — Test Multiple Connections

Use the SOCKS proxy to access more than one internal service.

For example:

```
proxychains curl http://<INTERNAL_IP>:8080
```

Then try another authorized service:

```
proxychains curl http://<INTERNAL_IP>:8000
```

Compare the results.

The goal is to understand that the same SOCKS proxy can
handle connections to different destination ports.

## Questions

Answer the following questions:

1. What does the `-D` option do?
2. What is a SOCKS proxy?
3. What is the purpose of port 1080?
4. Why can one SOCKS proxy handle different destinations?
5. What is the difference between `-L` and `-D`?
6. What happens when the SSH session is closed?
7. Why is a SOCKS proxy useful when accessing an internal network?

## Success Criteria

You should be able to explain:

`Application → SOCKS Proxy → SSH Tunnel → SSH Server → Destination`

You should understand that dynamic forwarding allows
multiple connections to be routed through a single SSH tunnel.
