# Lab 03 — SSH Remote Port Forwarding

## Difficulty

Beginner

## Objective

Understand how SSH remote port forwarding works.

The goal is to understand how a port on a remote machine
can forward traffic through an SSH connection to a service
on another machine.

## Scenario

You have two machines:

- Kali — test machine
- SSH Server — laboratory machine

Kali is running a TCP service on port 8080.

Your task is to create a remote SSH tunnel that makes
a port available on the SSH Server and forwards traffic
to the service on Kali.

## Topology

```
SSH Server:9000
       |
       | SSH tunnel
       |
       v
Kali:8080
```
## Tasks

### Task 1 — Start a TCP Service on Kali

On Kali, start a TCP listener on port 8080:

```
nc -lvnp 8080
```

Keep the terminal open.

### Task 2 — Verify SSH Access

From Kali, connect to the SSH Server:

```
ssh <USER>@<SSH_SERVER_IP>
```

Verify that you can successfully authenticate to the SSH Server.

### Task 3 — Create the Remote SSH Tunnel

From Kali, create a remote port forward:

```
ssh -R 9000:127.0.0.1:8080 <USER>@<SSH_SERVER_IP>
```

Keep the SSH session open.

The option `-R` creates a Remote Port Forward.

The remote port `9000` is connected through the SSH tunnel
to the TCP service running on Kali at port `8080`.

The traffic path is:

SSH Server:9000 → SSH tunnel → Kali:8080

### Task 4 — Test the Remote Tunnel

Open a new terminal on the SSH Server.

Test the remote port:

```
nc -vz 127.0.0.1 9000
```

If the tunnel is working, the connection should succeed.

The connection is forwarded through SSH to the TCP service
running on Kali at port 8080.

The traffic path is:

SSH Server:9000
       |
       | SSH tunnel
       v
Kali:8080

## Questions

Answer the following questions:

1. What does the `-R` option do?
2. Where is port 9000 opened?
3. Where is port 8080 running?
4. What happens when a connection reaches port 9000?
5. Why does the traffic reach Kali?
6. What happens if the SSH connection is closed?
7. What is the difference between `-L` and `-R`?
8. In what situation could remote port forwarding be useful?

## Success Criteria

You should be able to explain:

`SSH Server:9000 → SSH tunnel → Kali:8080`

You should understand that `-R` creates a remote
port forward and allows traffic from the remote side
to reach a service through the SSH connection.
