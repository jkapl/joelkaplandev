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