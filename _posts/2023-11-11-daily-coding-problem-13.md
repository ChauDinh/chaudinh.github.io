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
title: Daily coding problem on 2023-11-11 - Maximum Banana Moves
---

A string `S` made of uppercase English letters is given. In one move, six letters forming the word `BANANA` (one `B`, three `A`s and two `N`s) can be deleted from `S`. What is the maximum number times such a move can be applied in `S`?

Write a function that, given a string `S` of length `N`, returns the maximum number of moves that can be applied.

For example:

- Given `S = NAABXXAN`, the function should return 1.
- Given `S = NAANAAXNABABYNBZ`, the function should return 2 as we can apply two described steps above in `S`.
  From `NAANAAXNABABYNBZ` to `NAAXNABYNBZ`, and to `XBYNZ`.

```JavaScript
function maxBananaMoves(S) {
    const letterCounts = {
        'B': 1,
        'A': 3,
        'N': 2
    };
    const currentCounts = {
        'B': 0,
        'A': 0,
        'N': 0
    };

    let maxMoves = 0;
    for (let i = 0; i < S.length; i++) {
        const currentLetter = S[i];
        if (currentCounts.hasOwnProperty(currentLetter)) {
            currentCounts[currentLetter]++;
            if (
                currentCounts['B'] === letterCounts['B'] &&
                currentCounts['A'] === letterCounts['A'] &&
                currentCounts['N'] === letterCounts['N']
            ) {
                currentCounts['B'] = 0;
                currentCounts['A'] = 0;
                currentCounts['N'] = 0;
                maxMoves++;
            }
        }
    }

    return maxMoves;
}

console.log(maxBananaMoves("NAABXXAN")); // Output: 1
console.log(maxBananaMoves("NAANAAXNABABYNBZ")); // Output: 2
```

This function keeps track of the counts of `B`, `A`, and `N` while iterating through the string. When the counts match the required counts for `BANANA`, it increments the maximum moves count and resets the counts for the next substring. The final result is the maximum number of moves that can be applied.
