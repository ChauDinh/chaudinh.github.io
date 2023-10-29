---
layout: post
usehighlight: true
tags:
  [
    JavaScript,
    TypeScript,
    Data Structures,
    Algorithms,
    Coding Interview,
    Daily Coding Problem,
  ]
title: Daily coding problem on 2023-10-20 - Count of Smaller Numbers After Self
---

Given an integer array `nums`, return an integer array counts where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

<b>Example 1:</b>

Input: `nums` = [5,2,6,1]

Output: [2,1,1,0]

Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.

<b>Example 2:</b>

Input: `nums` = [-1]
Output: [0]

<b>Example 3:</b>

Input: `nums` = [-1,-1]
Output: [0,0]

<b>Constraints:</b>

1 <= `nums.length` <= 10^5

-10^4 <= `nums[i]` <= 10^4

You can find this problem on LeetCode platform and practice by implement using any programming language you want. I will implement with JavaScript. Let's think about a naive solution first.

Try to find a naive approach would be a best way to start, especially when we face hard problem.

<img src="https://drive.google.com/uc?id=1ZPSbeo74YIIyznCJu_J35w-13IYJJm44" width="300">

In the example 1, the first element is 5, we need to check all elements having index from 1 to 3 (2, 6, and 1) to count the number of elements smaller than 5. We repeat the progress for the second element and so on.

To repeat the progress, we need a loop that go throw entire elements of the original array. And at each index, for example `i`, we also need to loop throw the elements from index `i + 1` to index `length - 1` for counting. You can see that the time complexity of this algorithm is `O(n^2)` and since we don't use any extra memory or data structure, the space complexity should be `O(1)`.

<img src="https://drive.google.com/uc?id=1eqK1GEFhWtXFWVopX4tWZyPYi7HPEavr" width="300">

Here is an implementation of this algorithm with JavaScript:

```JavaScript
const findSmallers = function(nums, target, left, right) {
    if (nums.length === 0) return 0;

    let current = left;
    let result = 0;
    while (current >= left && current <= right) {
        if (nums[current] < target) {
            result++;
        }
        current++;
    }

    return result;
}

const countSmaller = function(nums) {
   if (nums.length === 0) return [];

   let counts = [];
   for (let i = 0; i < nums.length; i++) {
       const smallers = findSmallers(nums, nums[i], i + 1, nums.length - 1);
       counts.push(smallers);
   }

   return counts;
};
```

As you can see, this is a straightforward solution with two nested loops. The computational complexity is quadratic, so it does not pass on LeetCode platform (Time Limit Exceeded!). Now we know that `O(n^2)` is not fast enough, which complexity should we expect and seek for?

Linear time does not seem realistic because we need to search EVERY smaller element to the right of EVERY element in the original array. One loop may not be enough to get the expected result. So `O(n log n)` time complexity would be a better consideration. It also reminds us an famous algorithm related to binary tree, this is binary search tree (BST).

<img src="https://drive.google.com/uc?id=1fJIA9m2bHBE4HwLua8md5yH78XvTgvdg" width="150">

The image above represent the BST of input `nums` in example 1. But it's quite for us to imagine, so let's create a new one. We will create a BST for input `nums = [1, 2, 3, 4, 7, 3, 5, 6]`.

<img src="https://drive.google.com/uc?id=19Bv3fUp3sq7pK9IjFprZbal5Gqz3k_q-" width="500">

Coming soon...
