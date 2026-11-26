 # List Open Ports on a Linux VM with `ss`

## Goal
See which network ports are listening on my Linux machine, learn what each one is for, and match common port numbers to their services.

## Environment
- Host OS: Windows 11
- Linux: Ubuntu on WSL2
- Tool: `ss -tuln`

## Steps
1. Opened Ubuntu.
2. Ran `ss -tuln`.
   - `-t` TCP, `-u` UDP, `-l` listening sockets only, `-n` show numbers instead of names
3. Read each row: protocol, state, local address and port.
4. Checked which common ports (22, 80, 443) were not in the list.

## Screenshots
![ss -tuln output](ss-tuln.png)

## What I found

| Port | Protocol | Local address | Meaning |
|---|---|---|---|
| 53 | UDP and TCP | 127.0.0.53, 127.0.0.54, 10.255.255.254 | A small DNS helper running inside the VM |
| 323 | UDP | 127.0.0.1, ::1 | Likely the time-sync service control port (I did not verify which program) |

- `UNCONN` (UDP) and `LISTEN` (TCP) both mean the service is waiting for connections.
- `0.0.0.0:*` under Peer address means nobody is connected yet.
- Addresses starting with `127.` are loopback, so only programs inside this machine can connect to them. Another computer on my Wi-Fi cannot reach `127.0.0.53:53`.
- Ports 22 (SSH), 80 (HTTP) and 443 (HTTPS) were not listening, so the VM was not running an SSH or web server. Fewer open ports means a smaller attack surface.

## Common ports I practised

| Port | Service | Note |
|---|---|---|
| 22 | SSH | Secure remote terminal |
| 53 | DNS | Name lookups |
| 80 | HTTP | Plain web |
| 443 | HTTPS | Secure web |
| 3389 | RDP | Windows remote desktop |

## What I learned
- `ss -tuln` is a quick way to see what a machine is exposing to the network.
- A protocol (TCP or UDP) is how data travels. A service (DNS, SSH, web) is what the program does. Port 53 is the DNS service, and it uses UDP and TCP.
- Loopback addresses (`127.x.x.x`) are reachable only from the same machine.
- Checking for unexpected listening ports is a basic security step.

## Cleanup
- No cloud resources were created, so there is nothing to delete and no billing risk.
