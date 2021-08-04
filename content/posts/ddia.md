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

- LSM trees
  - Step 1
    - Append only log of operations (write, delete)
    - When log gets too big, split into sections and compact them (compaction in a background thread)
    - Keep an in-memory hash map of keys with file offsets in the log
  - Step 2
    - Keep keys in sorted order using in-memory data structure (red-black, avl tree). This is called a memtable
    - Keep a sparse index of 


- LSM tree vs B tree approach
  - LSM tree DBs often better at handling writes, worse at reads
    - For reads may need to check multiple memtables before finding value
    - However with bloom filters, can easily avoid unnecessary reads
    - Writes easier since using append only log
  - B trees better for reads, worse for writes (maybe)
    - Reads always log(n)
    - Writes need to rewrite entire memory page each time (check this)

- B Tree vs lists
  - O(1) insert on redis style database (ordered list)
  
- Cassandra, Dynamo
  - Multi master (any node can accept a write)

- In memory databases (Redis, Memcache)
  - Performance benefit not so much from keeping data in memory (since operating system caches frequently used pages anyway) but from not needed to encode in-memory data structures in a form that can be written to disk



## Replication

- Duplicate data
- Read replicas to increase read throughput
  - Most likely leads to eventual consistency


## Partitioning (sharding)

- Split data up
- Partition database (e.g. by geo) to enable higher write and read throughput
- Interesting issues around secondary indexes
  - Unless indexing by term (i.e. all red cars on a single partition), will require scatter-gather approach to certain queries

## Batch Processing

- Commonly used to build machine learning systems (classifiers) and recommendation systems
- Output is often a database (query user ID to retrieve suggested friends)
- Read-only key-value store is often the output, data file from batch job can be loaded in. Examples include Voldemort

## Conflict Resolution

- Generally try to avoid conflicts - use single leader replication
- Dynamo style databases have multi-leader by default; for these 

### Faulty but useful approaches
- Updates should have timestamps or random values, that way the system can have logic that sorts or discards older or newer data, or just randomly
- Resolve on write: some DBs allow for custom handlers (perl in Bucardo) to execute when conflicts are detected
- Resolve on read: CouchDB

### CRDTs (conflict free replicated datatypes)
- What the hell are these

## Big Ideas (maybe merge these later)

- Event based systems are useful because they contain faults by introducing asynchronous and independent systems. No distributed transactions
