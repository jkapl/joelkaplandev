+++ 
date = "2021-07-13" 
title = "Debugging Notes: Useful Tools" 
slug = "debugging-tools" 
tags = ["debugging", "tools"] 
categories = ["Tools"] 
series = ["Notes"] 
+++

## Methodology

Resource: [Linux Performance Tools, Brendan Gregg](https://www.youtube.com/watch?v=FJW8nGV4jxY)

# anti-methods
- streetlight: don't just use tools that are familiar 
- drunk man: fix random things

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
- CPU Profile (flame graph)
  - Understand all software > 1%

## Observability tools

Do your tools cover these parts of the system?

![Fill in](https://raw.githubusercontent.com/jkapl/joelkaplandev/master/static/observ_fill_blank.png?raw=true)

Some basic tools:

![Tools](https://raw.githubusercontent.com/jkapl/joelkaplandev/master/static/observ_tools_basic.png?raw=true)

### `uptime`
  - load averages. Very old, predates UNIX
  - only look at for 5s if load is close to 0.00
  - If load greater than num of cpus, indicates an issue
### `top`
  - System and per process summary
  - Can miss short lives processes
### `ps`
  - process status listing
  - `ps -ef f`
### `vmstat`
  - virtual memory statistics
  - vmstat [interval [count]]
### `iostat`
  - disk stats
### `mpstat`
  - per CPU stats
### `free`
### `sar`
  - system activity report
  - very effective way to explore many parts of the system
  - `sar -n DEV 1` : show activity from network (-n) devices (DEV)
  ![sar](https://raw.githubusercontent.com/jkapl/joelkaplandev/master/static/sar.png?raw=true)


## Networking: Tools

### Ping

### MTR
- MyTraceRoute
- Similar to `top` but for networking
- `mtr -4b google.com` - shows hops and packet loss, -4 for IPv4 (shows IPv6 by default), b for show IP addresses and hostnames

### `tcpdump`
- Show all packets on a particular port
- `sudo tcpdump -n -i any port 53` : show me all traffic on port 53 (DNS server lookup port) (-i for interface, here specifying any interface, -n for don't resolve names)
- `sudo tcpdump -i any port 1234` : show me all traffic on port 1234. Could show lots of SYN packets being sent but firewall blocking any connection
- sample output: `11:45:31.236938 IP 192.168.0.211.56426 > 192.168.0.1.53: 11330+ A? neopets.com. (29)`

### Network tab in the browser
- Timings tab shows timings of the request. Can check here if DNS lookups are taking too long for instance

## CPU

### top

### strace

## Networking: Techniques

### iptables

Resetting iptables counters can help determine if iptables rules are being triggered, and potentially causing an issue.
- Print all iptables rules: `sudo iptables-save`
- Show counters: `sudo iptables-save -c`
- Reset counters: `sudo iptables -Z` (clears default table) `sudo iptables -t nat -Z` (clears nat table)

### tcpdump

Could check if lots of repeat SYN packets are being sent - could indicate firewall blocking connection.