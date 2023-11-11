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
title: Daily coding problem on 2023-11-11 - Toeplitz matrix
---

In linear algebra, a Toeplitz matrix is one in which the elements on any given diagonal from top left
to bottom right are identical.

Here is an example:

1 | 2 | 3 | 4 | 8
5 | 1 | 2 | 3 | 4
4 | 5 | 1 | 2 | 3
7 | 4 | 5 | 1 | 2

Write a function to determine whether a given input matrix is a Toeplitz matrix.

The problem can be solved by applying a strategic method with 2 nested loops.

```JavaScript
function solution(matrix = []) {
  if (matrix.length === 0) return false;

  for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
      if (matrix[i][j] !== matrix[i + 1][j + 1]) return false;
    }
  }

  return true;
}
```
