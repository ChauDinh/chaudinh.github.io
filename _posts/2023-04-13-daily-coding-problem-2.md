---
layout: post
usehighlight: true
tags: [JavaScript, Graph, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-13 - Balance parentheses strings
---

This problem was asked by Google.

You're given a string consisting solely of (, ), and *. * can represent either a (, ), or an empty string. Determine whether the parentheses are balanced.

For example, `(()*` and `(*)` are balanced. `)*(` is not balanced.

This problem requires checking whether a string containing parentheses and asterisks is balanced, meaning that every opening parenthesis has a corresponding closing parenthesis, and all parentheses are properly nested.

To solve this problem, we can use a stack to keep track of the opening parentheses encountered so far. For each character in the string, we check if it is an opening parenthesis, a closing parenthesis, or an asterisk.

If the character is an opening parenthesis or an asterisk, we push it onto the stack.

If the character is a closing parenthesis, we check if the stack is empty or if the top element of the stack is an opening parenthesis. If either of these conditions is false, the parentheses are not balanced. Otherwise, we pop the top element from the stack.

After iterating through the entire string, we check if the stack is empty. If it is, the parentheses are balanced.

However, if the character is an asterisk, we need to consider two cases: either the asterisk is treated as a closing parenthesis, or it is treated as an empty string. Therefore, we need to keep track of two stacks: one for the opening parentheses and one for the asterisks.

We first process all opening parentheses and asterisks, pushing them onto their respective stacks. Then we process all closing parentheses, popping from either the opening parenthesis stack or the asterisk stack, in that order, to match the closing parenthesis. If neither stack has an element to pop, the parentheses are not balanced.

Here's the implementation of the same algorithm in JavaScript:

```JavaScript
function isBalancedParentheses(s) {
  const openingParents = [];
  const asterisks = [];

  for (let i = 0; i < s.length; i++) {
    const c = s.charAt(i);
    if (c === '(') {
      openingParents.push(i);
    } else if (c === '*') {
      asterisks.push(i);
    } else if (c === ')') {
      if (openingParents.length > 0) {
        openingParents.pop();
      } else if (asterisks.length > 0) {
        asterisks.pop();
      } else {
        return false;
      }
    }
  }

  while (openingParents.length > 0 && asterisks.length > 0) {
    if (openingParents[openingParents.length - 1] < asterisks[asterisks.length - 1]) {
      openingParents.pop();
      asterisks.pop();
    } else {
      return false;
    }
  }

  return openingParents.length === 0;
}
```
This code has a time complexity of `O(n)` and a space complexity of `O(n)`, where `n` is the length of the input string.

While the algorithm I described earlier uses a stack to keep track of the opening parentheses and asterisks encountered so far, it does not seem like an obvious candidate for a dynamic programming solution.

Dynamic programming is typically used to solve optimization problems by breaking them down into sub-problems and storing the solutions to these sub-problems in a table. However, the problem of checking whether a string of parentheses and asterisks is balanced does not seem to have an obvious optimization criterion that we can optimize using dynamic programming.

While it may not be the most efficient way to solve the problem, it is indeed possible to solve the problem using dynamic programming. Here's a dynamic programming solution. 

Let's define `dp[i][j]` to be a boolean that represents whether the substring from index i to index j is balanced.

Our base case is when we have a single character substring, `dp[i][i]` is true if the character is`*` or `(`, and false otherwise.

We can then fill in the rest of the table dp using the following recurrence relation:

```Python
if s[i] == '(' or s[i] == '*':
    dp[i][j] = dp[i+1][j]
if s[j] == ')' or s[j] == '*':
    dp[i][j] = dp[i][j-1]
if s[i] == '(' and s[j] == ')':
    dp[i][j] = dp[i+1][j-1]
if s[i] == '*' and s[j] == ')':
    dp[i][j] = dp[i+1][j-1]
if s[i] == '(' and s[j] == '*':
    dp[i][j] = dp[i+1][j] or dp[i][j-1]
if s[i] == '*' and s[j] == '*':
    dp[i][j] = dp[i+1][j] or dp[i][j-1]
```

Here's the complete JavaScript implementation of the dynamic programming solution:

```JavaScript
function isBalancedParentheses(s) {
  const n = s.length;
  const dp = new Array(n).fill(null).map(() => new Array(n).fill(false));
  for (let i = 0; i < n; i++) {
    if (s.charAt(i) === '*' || s.charAt(i) === '(') {
      dp[i][i] = true;
    }
  }
  for (let l = 2; l <= n; l++) {
    for (let i = 0; i <= n - l; i++) {
      const j = i + l - 1;
      if ((s.charAt(i) === '(' || s.charAt(i) === '*') && (s.charAt(j) === ')' || s.charAt(j) === '*')) {
        dp[i][j] = dp[i + 1][j - 1];
      }
      for (let k = i; k < j && !dp[i][j]; k++) {
        dp[i][j] = dp[i][k] && dp[k + 1][j];
      }
    }
  }
  return dp[0][n - 1];
}
```
This implementation follows the dynamic programming approach I described earlier. We use a 2D array dp to store the results of our sub-problems. We fill in the base cases where the substring length is 1, and then we fill in the rest of the table using the recurrence relation.

Note that we use a nested loop to iterate over all possible substrings of s and try to combine two smaller substrings to form a larger balanced substring. This results in a time complexity of `O(n^3)` and a space complexity of `O(n^2)`, which is much slower than the stack-based approach we discussed earlier.
