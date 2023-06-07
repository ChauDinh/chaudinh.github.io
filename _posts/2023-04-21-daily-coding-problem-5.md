---
layout: post
usehighlight: true
tags: [JavaScript, TypeScript, Dynamic Programming, Backtracking, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-21 - Subset sum
---

Given a list of integers S and a target number k, write a function that returns a subset of S that adds up to k. If such a subset cannot be made, then return null.

Integers can appear more than once in the list. You may assume all numbers in the list are positive.

For example, given `S = [12, 1, 61, 5, 9, 2]` and `k = 24`, return `[12, 9, 2, 1]` since it sums up to 24.

The problem described is a classic problem in computer science called the subset sum problem. There are several algorithms to solve this problem, including dynamic programming, backtracking, and branch and bound.

The dynamic programming approach involves building a table to store intermediate results and using them to compute the final result. The table is built row by row, and each row represents the subset sum for a subset of the input set of integers. The approach is similar to the knapsack problem, where we try to fill a knapsack with items of maximum value without exceeding its weight limit.

The backtracking approach involves recursively exploring all possible subsets of the input set of integers and checking if they add up to the target sum. This approach can be optimized using branch and bound, where we prune the search space by eliminating subsets that are guaranteed not to add up to the target sum.

Here's the high-level algorithm for the dynamic programming approach used in the above solution:

1. Initialize a table of size `(n+1) x (k+1)` with `false` values, where `n` is the length of the input set of integers and `k` is the target sum.

2. Set the value of `dp[i][0]` to true for all `0 <= i <= n`, since the empty set always adds up to 0.

3.  Iterate over each integer in the input set of integers and each possible sum from 1 to `k`. For each integer and sum, check if the integer can be included in the subset that adds up to the sum.

4. If the integer is too large to be included in the subset, set `dp[i][j]` to the value of `dp[i-1][j]`, which means the subset that adds up to the sum `j` does not include the integer.

5. If the integer can be included in the subset, set `dp[i][j]` to the value of `dp[i-1][j] || dp[i-1][j-S[i-1]]`, which means the subset that adds up to the sum `j` either includes the integer or not.

6. If `dp[n][k]` is `true`, there exists a subset of the input set of integers that adds up to `k`. Extract the subset by tracing back through the table starting from `dp[n][k]` and selecting the integers that were included in the subset.

7. If `dp[n][k]` is `false`, there is no subset of the input set of integers that adds up to `k`. Return `null`.



Here's the JavaScript/TypeScript implementation of the dynamic programming approach:

```TypeScript
function subsetSum(S: number[], k: number): number[] | null {
  const n = S.length;
  const dp: boolean[][] = [];

  // Initialize the table with false values
  for (let i = 0; i <= n; i++) {
    dp.push(new Array(k + 1).fill(false));
  }

  // Base case: the empty set adds up to 0
  for (let i = 0; i <= n; i++) {
    dp[i][0] = true;
  }

  // Fill the table row by row
  for (let i = 1; i <= n; i++) {
    for (let j = 1; j <= k; j++) {
      if (S[i - 1] > j) {
        // The current integer is too large to be included
        dp[i][j] = dp[i - 1][j];
      } else {
        // Include or exclude the current integer
        dp[i][j] = dp[i - 1][j] || dp[i - 1][j - S[i - 1]];
      }
    }
  }

  // Extract the subset that adds up to the target sum
  if (dp[n][k]) {
    const subset: number[] = [];
    let i = n;
    let j = k;

    while (i > 0 && j > 0) {
      if (dp[i - 1][j]) {
        // The current integer was not included
        i--;
      } else {
        // The current integer was included
        subset.push(S[i - 1]);
        j -= S[i - 1];
        i--;
      }
    }

    return subset;
  } else {
    return null;
  }
}
```
Here's how you can use the function:

```TypeScript
const S = [12, 1, 61, 5, 9, 2];
const k = 24;

const subset = subsetSum(S, k);

if (subset) {
  console.log(subset); // [12, 9, 2, 1]
} else {
  console.log('Subset not found');
}
```
In this implementation, the time complexity of the algorithm is `O(nk)`, where `n` is the length of the input set of integers and `k` is the target sum. The space complexity is also `O(nk)` because we need to store a table of that size to compute the intermediate results.

And here's the backtracking solution to the problem:

```TypeScript
function subsetSumBacktrack(S, k) {
  let subset = [];
  backtrack(S, k, subset, 0);
  return subset.length > 0 ? subset : null;
}

function backtrack(S, k, subset, idx) {
  if (k < 0) {
    return;
  }
  if (k === 0) {
    return true;
  }
  for (let i = idx; i < S.length; i++) {
    subset.push(S[i]);
    if (backtrack(S, k - S[i], subset, i + 1)) {
      return true;
    }
    subset.pop();
  }
  return false;
}
```
In the `subsetSumBacktrack` function, we initialize an empty `subset` array and call the `backtrack` function with the input set of integers `S`, target sum `k`, `subset` array, and starting index 0.

In the `backtrack` function, we check if the target sum `k` is less than 0, in which case we return since we have exceeded the target sum. If `k` is equal to 0, we have found a valid subset and return `true`.

We then loop through the input set of integers `S` starting from the index `idx`. We add the current integer to the `subset` array and recursively call the `backtrack` function with the updated target sum `k - S[i]`, the updated subset array, and the next index `i + 1`. If the recursive call returns true, we have found a valid subset and return true.

If the recursive call does not return true, we remove the last integer from the `subset` array and continue the loop to try the next integer.

If we exhaust all possible integers in the input set of integers `S` and still haven't found a valid subset, we return false.

The time complexity of this algorithm is exponential, which is `O(2^n)` and the space complexity is `O(n)`, where `n` is the length of the input set of integers.