---
layout: post
usehighlight: true
tags: [JavaScript, TypeScript, 2-D array, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-26 - Clockwise traversal in 2-D matrix
---

Given a `N` by `M` matrix of numbers, print out the matrix in a clockwise spiral.

For example, given the following matrix:

`[[1,  2,  3,  4,  5],
 [6,  7,  8,  9,  10],
 [11, 12, 13, 14, 15],
 [16, 17, 18, 19, 20]]`
You should print out the following:

1
2
3
4
5
10
15
20
19
18
17
16
11
6
7
8
9
14
13
12

The basic idea of the algorithm is to traverse the matrix in a clockwise spiral starting from the outermost layer and moving inward layer by layer. We use four variables to keep track of the boundaries of the current submatrix, and at each iteration, we traverse the `top`, `right`, `bottom`, and `left` sides of the submatrix and add each element to the result array. We then update the boundaries of the submatrix and repeat the process until all elements have been traversed.

1. Initialize the result array to an empty array.
2. Initialize four variables `top`, `bottom`, `left`, and `right` to represent the boundaries of the current submatrix, with top and left initially set to 0 and bottom and right initially set to the maximum row and column indices, respectively.
3. While `top` is less than or equal to `bottom` and `left` is less than or equal to `right`:
  3.1. Traverse the top row of the submatrix from left to right and add each element to the result array.
  3.2. Increment top to exclude the top row from the submatrix.
  3.3. Traverse the right column of the submatrix from top to bottom and add each element to the result array.
  3.4. Decrement right to exclude the right column from the submatrix.
1. If top is still less than or equal to bottom, traverse the bottom row of the submatrix from right to left and add each element to the result array.
2. Decrement bottom to exclude the bottom row from the submatrix.
3. If left is still less than or equal to right, traverse the left column of the submatrix from bottom to top and add each element to the result array.
4. Increment left to exclude the left column from the submatrix.
5. Return the result array.

Here is an implementation using TypeScript:

```TypeScript
function printSpiral(matrix: number[][]): void {
  if (!matrix) return;

  let topRow = 0;
  let bottomRow = matrix.length - 1;
  let leftCol = 0;
  let rightCol = matrix[0].length - 1;

  while (topRow <= bottomRow && leftCol <= rightCol) {
    // print top row
    for (let j = leftCol; j <= rightCol; j++) {
      console.log(matrix[topRow][j]);
    }
    topRow++;

    // print right column
    for (let i = topRow; i <= bottomRow; i++) {
      console.log(matrix[i][rightCol]);
    }
    rightCol--;

    // print bottom row
    if (topRow <= bottomRow) {
      for (let j = rightCol; j >= leftCol; j--) {
        console.log(matrix[bottomRow][j]);
      }
      bottomRow--;
    }

    // print left column
    if (leftCol <= rightCol) {
      for (let i = bottomRow; i >= topRow; i--) {
        console.log(matrix[i][leftCol]);
      }
      leftCol++;
    }
  }
}
```
The `printSpiral` function takes a 2D array (matrix) of numbers as input and prints out the elements in a clockwise spiral order. We use a while loop to iterate over the layers of the matrix, and four variables (`topRow`, `bottomRow`, `leftCol`, `rightCol`) to keep track of the boundaries of each layer.

For each layer, we print the elements in the top row, right column, bottom row, and left column in that order, and update the boundaries of the layer accordingly. We continue this process until we have printed all the layers.