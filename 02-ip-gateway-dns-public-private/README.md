# Finding My IP, Gateway and DNS (and Telling Private from Public)

After the first labs I knew my IP address and my gateway, but I hadn't looked at DNS yet. In this lab I found all three on my PC, worked out which addresses are private and which are public, and then looked up my public address to see how different it is from the one my PC uses.

## What I used
- Windows 11 on Wi-Fi
- Ubuntu on WSL2
- `ipconfig /all` and a browser IP lookup site

## Finding the DNS server
I ran `ipconfig /all` in Command Prompt and looked under the Wi-Fi adapter for the **DNS Servers** line. It showed `10.155.166.183`, plus an IPv6 address.

The first thing I noticed was that the DNS server is the **same address as my gateway**. At first that looked odd, but it's common. My router (or phone hotspot) also answers name lookups for me. DNS is basically the phone book of the internet: it turns a name like google.com into an IP address.

## Sorting my addresses into private and public
Next I labelled the four addresses I had so far:

| Address | What it is | Private or public? |
|---|---|---|
| 10.155.166.62 | My PC | Private |
| 10.155.166.183 | My gateway | Private |
| 10.155.166.183 | My DNS server (IPv4) | Private |
| 172.31.154.137 | My Linux (WSL) machine | Private |

All four are private. The ranges are:

| Range | From | To |
|---|---|---|
| 10.x.x.x | 10.0.0.0 | 10.255.255.255 |
| 172.16 to 172.31.x.x | 172.16.0.0 | 172.31.255.255 |
| 192.168.x.x | 192.168.0.0 | 192.168.255.255 |

I almost got caught by the 172 range. Only part of it is private, from 172.16 to 172.31, and my Linux address (172.31.x.x) sits right at the very end of it. I also forgot to mention the 192.168 range at first, which is the most common one at home.

## Looking at my public address
Then I searched "what is my IP address" in my browser. The address it showed started with **171**, which is outside all three private ranges, so it's a public IP. It was also completely different from my PC's address of 10.155.166.62.

So the picture looks like this:

```
My PC 10.155.166.62  -->  Gateway  -->  Public IP (171.x.x.x)  -->  Internet
(private address)         (the door)     (the address websites see)
```

My gateway swaps my private address for the public one on the way out. That trick is called NAT.
