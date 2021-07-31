+++
date = "2021-06-18"
title = "Refactoring - Notes"
slug = "refactoring-notes"
tags = ["refactoring", "software-design", "notes", "fowler"]
categories = []
series = ["Notes"]
+++

# Notes

- Pipelining
```
// return sum of even nums
const nums = [1,2,3]
const even = nums.filter(num => num % 2 == 0)
                 .reduce( (acc, curVal) => {
                   return acc + curVal
                  }, 0)
```