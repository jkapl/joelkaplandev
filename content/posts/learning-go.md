+++
date = "2021-05-26"
title = "Notes Taken While Learning Go"
slug = "learning-go"
tags = ["golang", "go", "programming", "notes"]
categories = []
series = ["Theme", "Hugo"]
+++

# Intro
- Strings, runes and range loops
  - Strings are arrays of bytes
  - Runes are integers representing UTF-8 code points
  - Ranging over a string will give you the rune value at each character
  - Indexing into a string will give you a byte
- Testing for existence in a map
  - `if map[key]` will give you the 0 value of the type if `key` doesn't exist (and 1 value if not)
  - if the value is a bool, this works nicely
  - if not, use initialization statement feature of `if`:
  ```
  if val, ok := map[key]; ok {
    // do more
  }
  ```

