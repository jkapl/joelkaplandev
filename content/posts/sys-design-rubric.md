+++ 
date = "2021-07-31" 
title = "Systems Design: A General Rubric" 
slug = "sys-design-rubric" 
tags = ["systems_design"] 
categories = ["systems_design"] 
series = ["Notes"] 
+++

## Functional Requirements

## Nonfunctional Requirements
- Consistency
- Availability
- Latency

## Estimates
- Number of users
- Data size (per request, total, per second, per hour, per day)

## Data Model

## API Endpoints
- POST: uploadVideo
- GET: viewFeed

## Bottlenecks
- Database usually the bottleneck
- Shard by geo
- Read only replicas
- Ways to work with hot keys (celebrity accounts, likes counters: see [Twitter](https://www.joelkaplan.dev/posts/real-life-sys-design-notes/), [Instagram](https://www.joelkaplan.dev/posts/real-life-sys-design-notes/))