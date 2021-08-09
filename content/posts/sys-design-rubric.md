+++ 
date = "2021-07-31" 
title = "Systems Design: A General Rubric" 
slug = "sys-design-rubric" 
tags = ["systems_design"] 
categories = ["systems_design"] 
series = ["Notes"] 
+++

## Functional Requirements
- Whatever the application should support 
- Example: upload videos, follow users, like videos, comment on videos

## Nonfunctional Requirements
- Consistency: is eventual consistency ok? Then we can probably use read replicas and support more read throughput
- Availability: Must be five 9's? Then will need multiple datacenters and potentially eventual consistency for replicatoin
- Latency: How many ms of latency? Will need CDN to reduce latency

## Estimates
- Number of users
- Data size (per request, total, per second, per hour, per day)

## Data Model

## API Endpoints
- POST: uploadVideo
- GET: viewFeed

## Bottlenecks
- Database usually the bottleneck
- Pick a database that will be most performant depending on the data access patterns (read v. write ratio, normalized or denormalized ok)
- Shard by geo
- Cache in front of DB
- Read only replicas
- Ways to work with hot keys (celebrity accounts, likes counters: see [Twitter](https://www.joelkaplan.dev/posts/real-life-sys-design-notes/), [Instagram](https://www.joelkaplan.dev/posts/real-life-sys-design-notes/))


# Examples

## TinyURL
500 million URLs /month
30 billion URLs * 100 bytes = 3 trillion bytes -> 3 billion KB -> 3 million MB -> 3,000 GB -> 3 TB
500M/month -> 20M/day -> 1M/hour -> 300/s. 10x peak -> 3K wps. One DB can handle
URL shortener service behind load balancer
Generating
Short, random and readable
Length: 7 chars, 62 chars to choose: [A-Z, a-z, 0-9]. This is base62 encoding.
To create a short link, store a global incrementing value and convert it to base 62, then increment it (if current value is 125, distribute that across 62^0, 62^1, etc. and then set values to
0:0, 1:1…10:a, 11:b…36:A, 37:B
Then always check if it’s already used just to be sure
62^7 = 3500 billion URLs = 110 years to run out
Could also do log62(30 billion) 62 to what power gives 30 billion, this should be length of shortlink
Read:write 100:1 then 30k rps
Need read replicas. 10k peaks means 300k rps
Mysql 10k selects per second so at least 3 read replicas
Caching layer for additional scaling. Redis can handle 100k rps
Hottest URLs 1% of total = 30 GB, can store each on Redis

## Uber
Driver connects via websocket to deliver location payload every 5s
latitude, longitude, direction, driver_id
500k active drivers, average shift of 6 hours, 4 shifts per day
125k drivers active at any moment, message every 5 seconds, 25k RPS on average
Peak x2 = 50k, x4 = 100k, x10 = 250k
Rule of thumb: 5k writes/s for any DB node
How to handle writing this location data?
Scale reads with read replicas, scale writes with sharding
Shard by geo (will get country hot spots)? custom areas (will need to maintain, update areas) - would need driver location service that takes coordinates of driver plus shard location service to find shard?
Use Uber H3 geolocation library (edge free) - hexagons of equal size, each mapped to a shard.
Riders
10M active
5 opens of app/day = 50M opens a day
Refresh driver locations every 5 s, average session is 1 minutes = 12 refreshes
50M * 12 = 600 M requests/day = ~7k requests per second
Postgres offers geolocation queries

