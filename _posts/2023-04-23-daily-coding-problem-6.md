---
layout: post
usehighlight: true
tags: [JavaScript, TypeScript, Dynamic Programming, Backtracking, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-23 - Longest increasing subsequence
---

This problem was asked by Facebook.

Describe an algorithm to compute the longest increasing subsequence of an array of numbers in `O(n log n)` time.

The problem of finding the longest increasing subsequence of an array of numbers is a classic problem in computer science. The problem can be solved using dynamic programming in `O(n^2)` time. However, we can solve the problem in `O(n log n)` time using an algorithm called Patience Sorting.

First, we see how to solve the problem using dynamic programming paradigm. 

The dynamic programming approach to solve this problem involves defining an array `dp` where `dp[i]` represents the length of the longest increasing subsequence ending at index `i` in the input array. We can compute the `dp` array using the following recurrence relation:

`dp[i] = max(dp[j]) + 1` for all `j` such that `0 <= j < i` and `nums[j] < nums[i]`

The length of the longest increasing subsequence is the maximum value in the `dp` array.

For instance, here's how we can compute the dp array for the input array `[3, 1, 4, 1, 5, 9, 2, 6, 5]`:

a. Initialize the `dp` array with all values set to 1.

`dp = [1, 1, 1, 1, 1, 1, 1, 1, 1]`


b. For each index `i` in the range 1 to `n-1`, compute the value of `dp[i]` using the recurrence relation:
   - For `i = 1`, we have `dp[1] = max(dp[j]) + 1` for all `j` such that `0 <= j < 1` and `nums[j] < nums[1]`. Since there is no such `j`, we have `dp[1] = 1`.
   - For `i = 2`, we have `dp[2] = max(dp[j]) + 1` for all `j` such that `0 <= j < 2` and `nums[j] < nums[2]`. The possible values of `j` are 0 and 1, and we have `nums[0] < nums[2]` and `nums[1] < nums[2]`, so `dp[2] = max(dp[0], dp[1]) + 1 = 2`.
   - For `i = 3`, we have `dp[3] = max(dp[j]) + 1` for all `j` such that `0 <= j < 3` and `nums[j] < nums[3]`. The possible values of `j` are 0, 1, and 2. We have `nums[0] < nums[3]` and `nums[2] < nums[3]`, so `dp[3] = max(dp[0], dp[2]) + 1 = 2`.
   - For `i = 4`, we have `dp[4] = max(dp[j]) + 1` for all `j` such that `0 <= j < 4` and `nums[j] < nums[4]`. The possible values of `j` are 0, 1, 2, and 3. We have `nums[0] < nums[4], nums[2] < nums[4]`, and `nums[3] < nums[4]`, so `dp[4] = max(dp[0], dp[2], dp[3]) + 1 = 3`.
   - For `i = 5`, we have `dp[5] = max(dp[j]) + 1` for all `j` such that `0 <= j < 5` and `nums[j] < nums[5]`. The possible values of `j` are 0, 1, 2, 3, and 4. We have `nums[0] < nums[5]`, `nums[2] < nums[5], nums[3] < nums[5]`, and `nums[4] < nums[5]`, so `dp[5] = max(dp[0], dp[2], dp[3], dp[4]) + 1 = 4`.
   - For `i = 6`, we have `dp[6] = max(dp[j]) + 1` for all `j` such that `0 <= j < 6` and `nums[j] < nums[6]`. The possible values of `j` are 0, 1, 2, 3, 4, and 5. We have `nums[0] < nums[6]`, `nums[2] < nums[6]`, `nums[3] < nums[6], nums[4] < nums[6]`, and `nums[5] < nums[6]`, so `dp[6] = max(dp[0], dp[2], dp[3], dp[4], dp[5]) + 1 = 5`.
   - For `i = 7`, we have `dp[7] = max(dp[j]) + 1` for all `j` such that `0 <= j < 7` and `nums[j] < nums[7]`. The possible values of `j` are 0, 1, 2, 3, 4, 5, and 6. We have `nums[0] < nums[7]` and `nums[6] < nums[7]`, so `dp[7] = max(dp[0], dp[6]) + 1 = 2`.
   - For `i = 8`, we have `dp[8] = max(dp[j]) + 1` for all `j` such that `0 <= j < 8` and `nums[j] < nums[8]`. The possible values of `j` are 0, 1, 2, 3, 4, 5, 6, and 7. We have `nums[0] < nums[8]`, `nums[2] < nums[8]`, `nums[3] < nums[8]`, `nums[4] < nums[8]`, `nums[5] < nums[8]`, and `nums[6] < nums[8]`, so `dp[8] = max(dp[0], dp[2], dp[3], dp[4], dp[5], dp[6]) + 1 = 5`.


c. The length of the longest increasing subsequence is the maximum value in the `dp` array, which is 5 in this case.

Therefore, the longest increasing subsequence of the input array `[3, 1, 4, 1, 5, 9, 2, 6, 5]` is `[1, 4, 5, 9]`.

The time complexity of this algorithm is `O(n^2)`, because we perform n iterations of a nested loop that takes `O(n)` time to execute. However, there is a more optimized version of this algorithm that uses binary search to reduce the time complexity to `O(n log n)`, which is also the requirement of the problem.

Here's how the algorithm works with the same input array:

1. Create an empty list called `tails`.
2. For each number in the input array, find the tail where the number can be placed using binary search. The tail where the number can be placed is the tail with the smallest top number greater than or equal to the current number. If there is no such tail, create a new tail.
3. Place the number on top of the tail found in step 2.
4. The length of the longest increasing subsequence is the number of tails created.

Let's illustrate this algorithm with an example. Consider the input array `[3, 1, 4, 1, 5, 9, 2, 6, 5]`.

We start with an empty list of tails.

-Step 1: Create an empty list called tails.

tails: []

- Step 2: For the first number (3), find the tail where it can be placed using binary search. Since there is no tail with a top number greater than or equal to 3, we create a new tail.

tails: [3]

- Step 3: For the second number (1), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 1 is 3, so we place 1 on top of that tail.

tails: [1, 3]

- Step 4: For the third number (4), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 4 is 5, so we create a new tail.

tails: [1, 3, 4]

- Step 5: For the fourth number (1), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 1 is 3, so we place 1 on top of that tail.

tails: [1, 1, 4]

- Step 6: For the fifth number (5), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 5 is 9, so we create a new tail.

tails: [1, 1, 4, 5]

- Step 7: For the sixth number (9), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 9 is not found, so we create a new tail.

tails: [1, 1, 4, 5, 9]

- Step 8: For the seventh number (2), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 2 is 3, so we place 2 on top of that tail.

tails: [1, 1, 2, 5, 9]

- Step 9: For the eighth number (6), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 6 is 9, so we create a new tail.

tails: [1, 1, 2, 5, 9, 6]

- Step 10: For the ninth number (5), find the tail where it can be placed using binary search. The smallest top number greater than or equal to 5 is 6, so we place 5 on top of that tail.

tails: [1, 1, 2, 5, 9, 6, 5]

The length of the longest increasing subsequence is the number of tails created, which is 6 in this case.

Thus, the longest increasing subsequence of the input array [3, 1, 4, 1, 5, 9, 2, 6, 5] is [1, 4, 5, 9].

The time complexity of this algorithm is `O(n log n)` because each binary search takes `O(log n)` time, and we perform it n times, once for each number in the input array. Additionally, the insertion of a number into a tail takes `O(1)` time. Therefore, the overall time complexity of this algorithm is `O(n log n)`.

```TypeScript
function lengthOfLIS(nums: number[]): number {
  const tails: number[] = [];

  for (const num of nums) {
    if (tails.length === 0 || num > tails[tails.length - 1]) {
      tails.push(num);
    } else {
      let left = 0;
      let right = tails.length - 1;
      while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (tails[mid] < num) {
          left = mid + 1;
        } else {
          right = mid;
        }
      }
      tails[left] = num;
    }
  }

  return tails.length;
}
```

This implementation uses a `tails` array to store the tails of increasing subsequences. For each number in the input array, it either appends the number to `tails` if it is greater than the last element in `tails`, or uses binary search to find the index of the smallest element in `tails` that is greater than or equal to the number and replaces that element with the number. Finally, it returns the length of `tails`, which is the length of the longest increasing subsequence.