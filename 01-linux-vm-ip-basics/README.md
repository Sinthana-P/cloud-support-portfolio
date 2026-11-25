# Linux VM Setup and IP Basics

## Goal
Set up a Linux virtual machine and learn to read its network configuration.

## Environment
- Host OS: [Windows 11]
- Virtualization: [VirtualBox / WSL2]
- Guest OS: [Ubuntu 24.04]

## Steps
1. Installed [VirtualBox / WSL] and created an Ubuntu VM.
2. Opened a terminal in the VM.
3. Ran `ip a` to see the IP address and network interface.
4. Ran `ip route` to find the default gateway.
5. Ran `ipconfig /all` on the Windows host to compare. 

## Screenshots
![ip a output](ip-a.png)
![ip route output](ip-route.png)

## What I found
| Item | Value | Private or public? |
|---|---|---|
| VM IP address | 10.0.2.15 | Private |
| Default gateway | 10.0.2.2 | Private |
| DNS server | [your value] | [Private/Public] |

## What I learned
- `ip a` shows the interface name and IP address of the machine.
- `10.x.x.x`, `172.16–31.x.x` and `192.168.x.x` are private ranges.
- The gateway is the router the VM uses to reach other networks.
- [Add one thing that confused you and how you solved it.]
