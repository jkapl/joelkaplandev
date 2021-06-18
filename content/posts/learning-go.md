+++
date = "2021-05-26"
title = "Notes Taken While Learning Go"
slug = "learning-go"
tags = ["golang", "go", "programming", "notes"]
categories = []
series = ["Notes"]
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
- Methods (actually receiver functions)
  - Methods are functions with a receiver
  - There's no need for a `this` keyword - just access either values from the object or the object itself with a pointer.
  - If the receiver is not an object (struct), then no pointer is needed to modify the values (I think)
```
type SomeInts []int

func (s SomeInts) sumInts () int {
  sum := 0
  for _, val := range s {
    sum += val
  }
  return sum
} 

func (s SomeInts) changeTheLastInt (i int) {
  s[len(s) - 1]=i
  fmt.Println(s)
}
```
  - If the receiver is a struct, then a pointer is needed:
```
  type SomeInts struct {
  myints []int
}

func (s SomeInts) sumInts () int {
  sum := 0
  for _, val := range s.myints {
    sum += val
  }
  return sum
} 

func (s *SomeInts) changeTheLastInt (i int) {
  s.myints[len(s.myints) - 1]=i
  fmt.Println(s.myints)
}
```
- Bit manipulation
  - Example of performance benefit from bit manipulation: multiplying by two - instead allocate space for `uint64` and left shift using `<<` operator:
```
num << 1
```
