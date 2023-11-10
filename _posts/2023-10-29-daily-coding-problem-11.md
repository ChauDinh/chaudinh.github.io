---
layout: post
usehighlight: true
tags:
  [
    JavaScript,
    Data Structures,
    Algorithms,
    Coding Interview,
    Daily Coding Problem,
  ]
title: Daily coding problem on 2023-10-30 - The shortest unique substring
---

Write a function solution that, given a string `S` of length `N`, returns the length of the shortest unique substring of `S`, that is, the length of the shortest word which occurs in `S` exactly once.

Examples:

1. Given `S = abaaba`, the function should return 2. The shortest unique substring of `S` is `aa`
2. Given `S = zyzyzyz`, the function should return 5. The shortest unique substring of `S` is `yzyzy`. Note that there are shorter words, like `yzy`, occurrences of which overlap, but they still count as multiple occurrences.
3. Given `S = aabbbabaaa`, the function should return 3. All substrings of size 2 occurs in `S` at least twice.

Assume that:

- `N` is an integer within the range [1..200]
- string `S` is made only of lowercase letters (a-z)
