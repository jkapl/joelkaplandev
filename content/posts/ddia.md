+++
date = "2021-06-18"
title = "Designing Data Intensive Applications - Notes"
slug = "ddia-notes"
tags = ["ddia", "distributed-systems", "notes"]
categories = []
series = ["Notes", "Books"]
+++

# Notes

## Database tradeoffs

- Data models
  - Lots of one-many relations: NoSQL DB
    - Think LinkedIn: one user, many jobs, schools. Read path not often in need of joins (all users by school)
  - Some many-many relations: SQL DB
  - Lots of many-many relations: Graph DB

- LSM tree vs B tree approach
  - LSM tree DBs often better at handling writes, worse at reads
    - For reads may need to check multiple memtables before finding value
    - Writes easier since using append only log
  - B trees better for reads, worse for writes (maybe)
    - Reads always log(n)
    - Writes need to rewrite entire memory page each time (check this)
  
- Cassandra, Dynamo
  - Multi master (any node can accept a write)



## Replication

- Read replicas to increase read throughput
  - Most likely leads to eventual consistency


## Partitioning

- Partition database (e.g. by geo) to enable higher write and read throughput
- Interesting issues around secondary indexes
  - Unless indexing by term (i.e. all red cars on a single partition), will require scatter-gather approach

## Batch Processing

- Commonly used to build machine learning systems (classifiers) and recommendation systems
- Output is often a database (query user ID to retrieve suggested friends)
- Key-value store is often the output, data file from batch job can be loaded in. Examples include Voldemort