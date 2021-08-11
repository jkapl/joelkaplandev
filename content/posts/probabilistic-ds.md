+++ 
date = "2021-08-11" 
title = "Probabilistic and On-Line Data Structures" 
slug = "probabilistic-on-line-data" 
tags = ["systems_design", "notes"] 
categories = ["systems_design"] 
series = ["Notes"] 
+++

- Probabilistic data structures 
  - Often used for real-time analytics and large datasets
  - Bloom filters for compact existence check:
    - Always tells if item is in set, slight change of false positives
    - bitmap
    - Check if a website is malicious (bloom filter of giant set of malicious sites)
  - hyperloglog
  - t-digest
    - [Ted Dunning talk](https://www.youtube.com/watch?v=CR4-aVvjE6A)
    - on-line accumuluation of rank-based statistics
    - sort of like a histogram with many quintiles, more accuracy in very high and very low quintiles, less in middle quintiles
    - variable accuracy, constant relative accuracy
    - computes clusters, large in middle, small towards end of distribution
    - continuously compute median (50th percentile) of a large number of integers from distributed nodes
    - order points, collect points together given scale of quintile, continually merging points, short buffer for new points, sort and merge
    - <100 nanosecond overhead per measurement
    - [under the hood uses avl trees](https://github.com/tdunning/t-digest/tree/main/core/src/main/java/com/tdunning/math/stats)
  - semigroups and monoids