+++ 
date = "2021-07-19" 
title = "Real Life Systems Design: Notes" 
slug = "real-life-sys-design-notes" 
tags = ["systems_design", "notes"] 
categories = ["systems_design"] 
series = ["Notes"] 
+++

## Slack architecture

### Database

- PHP web app delivers a payload with websocket URL, hash indicating when delivered, database shard for particular team
- DB sharded by team, main DB keeps track of team -> shard relation. Multiple replicated DBs for main and shards
  - main database: `SELECT db_shard FROM teams WHERE domain = %domain`
  - team specific shard: `SELECT * FROM channels WHERE team_id = 711`
- Master-master replication so that writes can always go through, even if one master is down. How to handle duplicate/conflicting writes? Teams configured to write to one master only (even numbered teams write to masterA, odd to masterB), if master fails then switch.
- Memcache for certain queries

### Message Server (web socket)

- Message server opens web socket connection with client. Also holds a 30 second buffer of messages to deliver once client joins web socket (if messages have been delivered between payload delivery and web socket connection)
- Also persists messages by writing to DB

### Async job queue

- Redis job queue for certain operations (link unfurling, search indexing, importing messages)

### NFRs/Bottleneck solutions
- Availability thru rate limiting guards in front of key services
- Monitor message queue depth in message server
- Master-master replication
- Shard DB by team

### Challenges
- Mains DB failure
- Quadratic relationship between members, teams, channels impacting start payload