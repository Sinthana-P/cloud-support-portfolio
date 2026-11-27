# Setting Up Linux on Windows (WSL) and Reading My Network Settings

I wanted a Linux machine to practise on, without the hassle of a full virtual machine. Windows 11 lets me run Linux inside Windows using WSL, so that's what I set up. Then I used `ip a` and `ipconfig` to see how both sides of my computer are connected to the network.

## Before I started
I checked that my PC could handle it. `winver` showed Windows 11 (version 25H2, build 26200), and Task Manager (Performance, then CPU) showed virtualization as **Enabled**. WSL needs both, so I was good to go.

## What I did
1. I opened PowerShell as administrator and ran `wsl --install`. It installed Ubuntu by default.
2. I restarted the PC when it asked. After the restart, Ubuntu opened and asked me to create a username and password for Linux.
3. In Ubuntu, I ran `ip a` to see the Linux side of the network.
4. In PowerShell, I ran `ipconfig` to see the Windows side.
5. Finally, I pinged Windows from Linux with `ping -c 4 172.31.144.1` to check that the two could talk to each other.

## Screenshots
![ip a in Ubuntu](ip-a.png)
![ipconfig in PowerShell](ipconfig.png)
![ping from Linux to Windows](ping-gateway.png)

## What I saw
In Ubuntu, `ip a` showed two interfaces. The first was `lo`, the loopback (127.0.0.1), which is the computer talking to itself. The second was `eth0`, the virtual network card, with the address **172.31.154.137/20**.

On the Windows side, `ipconfig` showed my Wi-Fi address **10.155.166.62** with the mask 255.255.255.0 and the default gateway **10.155.166.183**. It also listed a virtual adapter for WSL with the address **172.31.144.1**.

That last one was the interesting part. My Linux address (172.31.154.137) sits on the same network as 172.31.144.1, so Windows is acting as Linux's gateway. WSL basically gives Linux its own small virtual network inside my PC, and Windows is the door out of it.

All of these addresses are private ones. They start with 10 or 172.16 to 172.31, which are ranges that are never used on the public internet.

## The ping test
`ping -c 4 172.31.144.1` from Ubuntu worked perfectly: 4 packets sent, 4 received, **0% packet loss**. The replies took about 0.5 to 0.9 milliseconds, which makes sense because the "other computer" is really just Windows on the same machine.

I also noticed the replies had a TTL of 128. Windows starts at 128 and Linux usually starts at 64, so that's a small hint that the reply really came from Windows.

## What I took away from this
- WSL runs Linux inside Windows and is much lighter than a full virtual machine.
- `ip a` on Linux and `ipconfig` on Windows show the same kind of information: IP address, subnet mask and gateway.
- Linux inside WSL lives on its own little virtual network, with Windows as its gateway.
- Private ranges are 10.x.x.x, 172.16 to 172.31.x.x and 192.168.x.x.
- Ping with low response times and 0% loss means the path between two machines is healthy.

## Cleanup
Nothing to delete here. I didn't create any cloud resources, so there was no billing risk.
