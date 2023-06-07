---
layout: post
usehighlight: true
tags: [JavaScript, TypeScript, Binary Search Tree, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-27 - Largest node in binary search tree
---

Given the root to a binary search tree, find the second largest node in the tree.

To find the second largest node in a binary search tree, we can use the fact that in a binary search tree, the largest element is the rightmost element, and the second largest element is either the parent of the largest element or the largest element in the left subtree.

Here is an algorithm to find the second largest node in a binary search tree:

- Start at the root of the tree.
- Traverse down the right subtree until we reach a node that has no right child. This node contains the largest element in the tree.
- If the node found in step 2 has a left subtree, the second largest element is the largest element in the left subtree. Traverse down the left subtree, following the right child pointers, until we reach a node with no right child. This node contains the largest element in the left subtree, which is the second largest element in the entire tree.
- If the node found in step 2 does not have a left subtree, the second largest element is the parent of this node. Traverse up the tree, following the parent pointers, until we reach a node that is the left child of its parent. This parent node is the second largest element in the tree.

```TypeScript
class TreeNode {
  value: number;
  left: TreeNode | null;
  right: TreeNode | null;

  constructor(value: number) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

function findSecondLargest(root: TreeNode | null): TreeNode | null {
  if (root === null) {
    return null;
  }

  let parent: TreeNode | null = null;
  let node: TreeNode = root;

  // Traverse down the right subtree until we reach a node that has no right child
  while (node.right !== null) {
    parent = node;
    node = node.right;
  }

  // If the node found in step 2 has a left subtree, find the largest element in the left subtree
  if (node.left !== null) {
    node = node.left;
    while (node.right !== null) {
      node = node.right;
    }
    return node;
  }

  // If the node found in step 2 does not have a left subtree, return its parent
  return parent;
}
```
In this implementation, we keep track of the parent node while traversing down the right subtree. When we reach the largest node, we use the `parent` variable to keep track of its parent. If the largest node has a left subtree, we follow the same procedure as before to find the largest node in the left subtree, which is the second largest node in the tree. If the largest node does not have a left subtree, we simply return its parent, which is the second largest node in the tree.

The time complexity of the algorithm to find the second largest node in a binary search tree is `O(h)`, where `h` is the height of the tree. In the worst case, when the binary search tree is completely unbalanced and resembles a linked list, the height of the tree is equal to the number of nodes, and the time complexity is `O(n)`, where n is the number of nodes in the tree.

The space complexity of the algorithm is `O(1)`, since we're not using any additional data structures to store information about the nodes.

