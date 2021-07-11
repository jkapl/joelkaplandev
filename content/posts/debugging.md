+++ 
date = "2021-06-29" 
title = "Debugging Notes: Useful Tools" 
slug = "debugging-tools" 
tags = ["debugging", "tools"] 
categories = ["Tools"] 
series = ["Notes"] 
+++

## Networking

### Ping

### MTR
- MyTraceRoute
- Similar to `top` but for networking
- `mtr -4b google.com` - shows hops and packet loss, -4 for IPv4 (shows IPv6 by default), b for show IP addresses and hostnames

### tcpdump

- Show all packets on a particular port
- `sudo tcpdump -n -i any port 53` : show me all traffic on port 53 (DNS server lookup port)
- sample output: `11:45:31.236938 IP 192.168.0.211.56426 > 192.168.0.1.53: 11330+ A? neopets.com. (29)`

### Networking tab in the browser
- Timings tab shows timings of the request