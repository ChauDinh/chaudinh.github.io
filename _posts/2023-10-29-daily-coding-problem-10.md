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
title: Daily coding problem on 2023-10-29 - Calculate the number of pairs can be selected
---

There is an array of `numbers` made of N integers. Each number has at least two digits and its first and last digits are different. You can select a pair of numbers if the last digit of the first selected number is the same as the first digit of the second selected number. Calculate the number of ways in which such a pair of numbers can be selected.

Write a function `function  solution(numbers)` that given an array `numbers` made of N integers, returns the number of ways to select a pair of numbers as described above.

Examples:

1. Given `numbers = [30, 12, 29, 91]`, the function should return 3. The pairs are: (12, 29), (29, 91) and (91, 12).
2. Given `numbers = [122, 21, 21, 23]`, the function should return 5. The pairs are: (122, 21) occurring twice, (122, 23) and (21, 122) occurring twice. Please note that the same pair of numbers can appear multiple times if teh pairs of their indices are different.

Write an efficient algorithm for the following assumptions:

- N is an integer within the range [1..100000]
- each element of array `numbers` is an integer within the range [10..1000000000]
- the first and last digits in each element of array `number` are different
- the answer does not exceed 1000000000.
