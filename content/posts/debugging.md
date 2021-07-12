+++ 
date = "2021-06-29" 
title = "Debugging Notes: Useful Tools" 
slug = "debugging-tools" 
tags = ["debugging", "tools"] 
categories = ["Tools"] 
series = ["Notes"] 
+++

## Methodology

# anti-methods
- streetlight: don't just use tools that are familiar 
- drunk man: fix random things
- 

# methods
- problem statement
  - What makes you think there is an issue? 
  - Has the system ever performed well?
  - What has changed recently?
  - Is the performance issue in latency or
- workload characterization
  1. Who is causing the load? PID, UID, IP address
  2. Why is the load called? code path, stack trace
  3. What is the load? IOPS, tput, type r/w
  4. How is the load changing over time?  
- USE method
  - For every resource, check:
    1. Utilization (busy time)
    2. Saturation (queue length)
    3. Errors
  - Helps to have functional (block) diagram of system
- off-CPU analysis


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

## CPU

### top

### strace