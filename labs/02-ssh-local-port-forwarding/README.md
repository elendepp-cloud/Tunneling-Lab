# Lab 02 — SSH Local Port Forwarding

## Difficulty

Beginner

## Objective

Understand how SSH local port forwarding works.

The goal is to understand how a local port can be connected
to a remote service through an SSH connection.

## Scenario

You have two machines:

- Kali — test machine
- SSH Server — laboratory machine

The SSH Server can access a TCP service on port 8080.

Your task is to create an SSH tunnel and access this service
through a local port on Kali.

## Topology

```
Kali
  |
  | localhost:9000
  |
  v
SSH Tunnel
  |
  v
SSH Server
  |
  v
Internal Service :8080
```

### Task 1 — Verify SSH Access

From Kali, connect to the SSH Server:

`ssh <USER>@<SSH_SERVER_IP>`

Verify that you can successfully authenticate to the SSH Server.

### Task 2 — Verify the Internal Service

On the SSH Server, check whether the internal TCP service is reachable:

```
nc -vz <INTERNAL_IP> 8080
```

The connection should succeed if the service is running.


### Task 3 — Create the SSH Tunnel

From Kali, create a local port forward:

```
ssh -L 9000:<INTERNAL_IP>:8080 <USER>@<SSH_SERVER_IP>
```

Keep the SSH session open.

The option `-L` creates a Local Port Forward.

The local port `9000` will be forwarded through the SSH connection
to `<INTERNAL_IP>:8080`.

The tunnel can be represented as:

Kali:9000 → SSH Server → Internal Service:8080

### Task 4 — Test the Tunnel

Open a new terminal on Kali.

Test the local port:

```
nc -vz 127.0.0.1 9000
```

If the tunnel is working, the connection should succeed.

The traffic path is:

Kali:9000
   |
   | SSH tunnel
   v
SSH Server
   |
   v
Internal Service:8080

## Questions

Answer the following questions:

1. What does the `-L` option do?
2. What is the purpose of port 9000?
3. What is the purpose of port 8080?
4. Where does the SSH connection exist?
5. Why can Kali connect to `127.0.0.1:9000`?
6. What happens to the traffic after it reaches port 9000?
7. What happens if the SSH tunnel is closed?
8. What is the difference between a direct connection and a tunneled connection?

## Success Criteria

You should be able to explain:

`Kali:9000 → SSH tunnel → SSH Server → Internal Service:8080`

You should understand that port 9000 is the local entry point
to the tunnel, while port 8080 is the destination service.
