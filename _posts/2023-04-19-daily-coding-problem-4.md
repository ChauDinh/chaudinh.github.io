---
layout: post
usehighlight: true
tags: [JavaScript, Bitwise, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-15 - Compare without using direct comparisons, branching and if/else
---

This question is asked by Nvidia: Find the maximum of two numbers without using any if-else statements, branching, or direct comparisons.

Here is an implementation using bitwise operations:

```JavaScript
function max(a, b) {
    let diff = a - b;
    let mask = diff >> 31;
    let max = a + mask * diff;
    return max;
}
```

Note that the code above is very similar to C++ solution. Bitwise operations in JavaScript are performed using the bitwise logical operators `|`, `&`, `^`, `~`, `<<`, and `>>`. The behavior of these operators is similar to the C++ operators, but the syntax is slightly different. The right shift operator `>>` is used for arithmetic shift in both C++ and JavaScript, so it can be used to extract the sign bit of the difference `a - b` in JavaScript as well.

This code does not use any if-else statements, branching, or direct comparisons, but uses bitwise operations to compute the maximum of two numbers.

Here's a step-by-step explanation of how the code works:

1. We define a function called `max` that takes two arguments, `a` and `b`, which are the numbers we want to compare.

2. We subtract `b` from `a` to obtain the difference between the two numbers, and store it in a variable called `diff`. The value of `diff` is positive if `a` is greater than `b`, and negative if `a` is less than `b`.

3. We use a bitwise right shift operator `>>` to extract the sign bit of `diff`. This sign bit is either `0` or `1`, depending on whether `diff` is positive or negative. We store the sign bit in a variable called `mask`.

4. We compute the maximum of `a` and `b` by adding `mask * diff` from `a`. If `mask` is 0, then `diff` is negative, so `mask * diff` is 0, and we get a + 0, which is a. If `mask` is 1, then `diff` is positive, so `mask * diff` is diff, and we get `a + diff`, which is `b`.

5. We return the value of max, which is the maximum of `a` and `b`. 

Note that this solution assumes that x and y are 32-bit signed integers. It may not work correctly for unsigned integers or other data types.

We still have another approach with absolute value:

1. We compute the sum of a and b, which is (a + b).

2. We compute the absolute difference between a and b, which is `|a - b|`.

3. We add the absolute difference to the sum of a and b, which gives `(a + b) + |a - b|`. This value is always greater than or equal to a and b.

4. We divide this value by 2 to obtain the maximum of a and b.

```C++
int max(int a, int b) {
    return (a + b + abs(a - b)) / 2;
}
```

Note that this solution assumes that `a` and `b` are 32-bit signed integers. It may not work correctly for unsigned integers or other data types. Also, the use of `abs` function may not be allowed in some interviews as it could be considered as a direct comparison. In that case, you could use the bitwise operations solution that I provided earlier. Alternatively, you could implement your own absolute value function using bitwise operations, like this:

```C++
int my_abs(int x) {
    int mask = x >> 31;
    return (x ^ mask) - mask;
}
```
Here's how it works:

1. We use a bitwise right shift operator `>>` to shift the sign bit of `x` to the least significant bit position, so that the sign bit becomes either 0 or 1. This sign bit is stored in the variable `mask`.

2. We use a bitwise XOR operator `^` to flip all the bits of `x` if `mask` is 1 (i.e., if x is negative). This gives us the two's complement of `x`, which is equivalent to its absolute value.

3. We subtract `mask` from the result to undo the effect of the sign bit. If `x` is negative, `mask` is 1 and the `result` is `~x + 1`. If `x` is non-negative, `mask` is 0 and the result is `x - 0`.

4. We return the result, which is the absolute value of `x`. 

This implementation uses the `my_abs` function compute the absolute difference between `a` and `b`, and then adds it to the sum of `a` and `b`. Finally, it divides the result by 2 to get the maximum of the two numbers.

```JavaScript
function max(a, b) {
    return (a + b + my_abs(a - b)) / 2;
}
```