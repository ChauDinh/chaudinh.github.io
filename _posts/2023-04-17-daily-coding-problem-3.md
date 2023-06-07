---
layout: post
usehighlight: true
tags: [JavaScript, Binary Tree, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-14 - The maximum path between two nodes in binary tree
---

Given a binary tree of integers, find the maximum path sum between two nodes. The path must go through at least one node, and does not need to go through the root.

So the problem asked about binary tree data structure, I want to mention the differences between binary tree and binary search tree first:

A binary tree is a tree data structure in which each node has at most two children, referred to as the left child and the right child. A binary search tree, on the other hand, is a binary tree with the following additional property: for any node n, all nodes in its left subtree have values less than n's value, and all nodes in its right subtree have values greater than n's value.

This property makes binary search trees particularly useful for searching and sorting operations. For example, to search for a value x in a binary search tree, we start at the root node and compare x to the node's value. If x is less than the node's value, we continue the search in the left subtree. If x is greater than the node's value, we continue the search in the right subtree. If x is equal to the node's value, we have found the value we were looking for.

The worst-case time complexity for searching in a binary search tree is O(h), where h is the height of the tree. In the worst case, the height of the tree can be O(n), where n is the number of nodes in the tree, which makes the worst-case time complexity O(n). However, if the binary search tree is balanced, which means the height is O(log n), then the worst-case time complexity is O(log n).

In summary, while both binary trees and binary search trees have similar structures, the additional property of a binary search tree makes it useful for searching and sorting operations with a potentially better worst-case time complexity than a regular binary tree.

Back to the problem, we can use a recursive approach where we calculate the maximum path sum for each node in the tree. For each node, we need to calculate the maximum path sum that goes through that node and ends at one of its child nodes. We also need to calculate the maximum path sum that goes through that node and does not end at one of its child nodes.

We can define a helper function that takes a node as input and returns a tuple containing two values: the maximum path sum that goes through the node and ends at one of its child nodes, and the maximum path sum that goes through the node and does not end at one of its child nodes. Here is the JavaScript implementation for finding the maximum path sum in a binary tree:

```JavaScript
class TreeNode {
  constructor(val, left=null, right=null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

class Solution {
  constructor() {
    this.max_sum = -Infinity;
  }

  maxPathSum(root) {
    this.helper(root);
    return this.max_sum;
  }

  helper(node) {
    if (!node) {
      return 0;
    }

    const left_max = Math.max(0, this.helper(node.left));
    const right_max = Math.max(0, this.helper(node.right));

    const max_ending = node.val + Math.max(left_max, right_max);
    const any_path = node.val + left_max + right_max;
    this.max_sum = Math.max(this.max_sum, any_path);

    return max_ending;
  }
}
```
The time complexity of this algorithm is `O(n)`, where `n` is the number of nodes in the binary tree, since we visit each node exactly once. The space complexity is `O(h)`, where `h` is the height of the binary tree, since the maximum depth of the recursive call stack is `h`.

Here's a non-recursive approach to finding the maximum path sum in a binary tree using a stack:

```JavaScript
class TreeNode {
  constructor(val, left=null, right=null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

class Solution {
  maxPathSum(root) {
    let max_sum = -Infinity;
    const stack = [root];
    const left_max = new Map();
    const right_max = new Map();
    left_max.set(null, 0);
    right_max.set(null, 0);

    while (stack.length > 0) {
      const node = stack[stack.length-1];
      if (node.left && !left_max.has(node.left)) {
        stack.push(node.left);
      } else if (node.right && !right_max.has(node.right)) {
        stack.push(node.right);
      } else {
        stack.pop();
        const left_val = left_max.get(node.left);
        const right_val = right_max.get(node.right);
        const max_ending = Math.max(node.val, node.val + Math.max(left_val, right_val));
        const any_path = node.val + left_val + right_val;
        max_sum = Math.max(max_sum, any_path);
        left_max.set(node, max_ending);
        right_max.set(node, max_ending);
      }
    }

    return max_sum;
  }
}
```

The algorithm uses a stack to traverse the binary tree in a depth-first manner without recursion. At each node, we check if its left and right child nodes have been visited before. If not, we push them onto the stack. If both children have been visited, we pop the node from the stack and calculate the maximum path sum that goes through that node. We also update the maximum path sum seen so far.

The time complexity of this algorithm is also `O(n)`, where `n` is the number of nodes in the binary tree, since we visit each node exactly once. The space complexity is ```O(n)```, since in the worst case scenario the stack will hold all the nodes of the binary tree.

The time complexity of the recursive and non-recursive solutions provided earlier is already optimal at `O(n)`, where n is the number of nodes in the binary tree, because we need to visit each node exactly once.

However, we can make a small optimization to the recursive solution to reduce the space complexity from `O(h)` to `O(1)`, where `h` is the height of the binary tree. Instead of storing the maximum path sum that goes through each node as a tuple in the helper function, we can use a class variable to keep track of the maximum path sum seen so far. Here's the modified code:

```JavaScript
class TreeNode {
  constructor(val, left=null, right=null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

class Solution {
  constructor() {
    this.max_sum = -Infinity;
  }

  maxPathSum(root) {
    this.helper(root);
    return this.max_sum;
  }

  helper(node) {
    if (!node) {
      return 0;
    }

    const left_max = Math.max(0, this.helper(node.left));
    const right_max = Math.max(0, this.helper(node.right));

    const max_ending = node.val + Math.max(left_max, right_max);
    const any_path = node.val + left_max + right_max;
    this.max_sum = Math.max(this.max_sum, any_path);

    return max_ending;
  }
}
```

This modified solution has a space complexity of `O(1)` because we only use a constant amount of space to store the maximum path sum seen so far. The time complexity is still `O(n)` because we visit each node exactly once.