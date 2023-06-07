---
layout: post
usehighlight: true
tags: [JavaScript, Graph, Data Structures, Algorithms, Coding Interview, Daily Coding Problem]
title: Daily coding problem on 2023-04-12 - Hamilton circle
---

Given a list of words, determine whether the words can be chained to form a circle. A word `X` can be placed in front of another word `Y` in a circle if the last character of `X` is same as the first character of `Y`.

For example, the words ['chair', 'height', 'racket', touch', 'tunic'] can form the following circle: chair --> racket --> touch --> height --> tunic --> chair

This is a common problem in graph theory, specifically in the field of Hamiltonian cycles. One approach to solve this problem is to represent the list of words as a graph, where each word is a vertex, and an edge exists between two vertices if the last character of one word matches the first character of another word. Then, we can search for a Hamiltonian cycle in this graph, which would represent a valid chain of words.

Here's a possible algorithm to solve this problem:

1. Create an empty graph `G` with vertices representing each word in the list.
2. For each pair of words `(w1, w2)` in the list, check if the last character of `w1` matches the first character of `w2`. If so, add an edge from w1 to `w2` in `G`.
3. Check if `G` contains a Hamiltonian cycle. If so, return `true`, otherwise return `false`.

There are various algorithms to find a Hamiltonian cycle in a graph, such as brute-force search, backtracking, and heuristic approaches. One of the simplest algorithms is based on a depth-first search (DFS) with pruning, which can efficiently find a Hamiltonian cycle in many cases.

Here's an implementation of the algorithm in JavaScript:

```JavaScript
function canFormCircle(words) {
  const graph = buildGraph(words);
  const visited = new Set();
  const start = words[0];
  return dfs(graph, visited, start, start);
}

function buildGraph(words) {
  const graph = new Map();
  for (const word of words) {
    graph.set(word, new Set());
  }
  for (let i = 0; i < words.length; i++) {
    for (let j = 0; j < words.length; j++) {
      if (i !== j && words[i][words[i].length - 1] === words[j][0]) {
        graph.get(words[i]).add(words[j]);
      }
    }
  }
  return graph;
}

function dfs(graph, visited, curr, start) {
  visited.add(curr);
  if (visited.size === graph.size && graph.get(curr).has(start)) {
    return true;
  }
  for (const neighbor of graph.get(curr)) {
    if (!visited.has(neighbor)) {
      if (dfs(graph, visited, neighbor, start)) {
        return true;
      }
    }
  }
  visited.delete(curr);
  return false;
}
```

The `canFormCircle` function takes a list of words as input and returns `true` if the words can be chained to form a circle, or `false` otherwise.

The `buildGraph` function creates a graph representation of the words using a `Map` object, where each key is a word and each value is a `Set` of words that can follow it. It does this by iterating over all pairs of words in the list and checking if the last character of one word matches the first character of the other word.

The `dfs` function performs a depth-first search of the graph to find a Hamiltonian cycle. It takes the graph, a set of visited vertices, the current vertex being visited, and the starting vertex as input. It recursively visits all neighbors of the current vertex that haven't been visited yet, and returns true if a Hamiltonian cycle is found (i.e., all vertices have been visited and the last vertex has an edge to the starting vertex).

The time complexity of this algorithm is `O(n^2 * w)`, where `n` is the number of words and `w` is the maximum length of a word. The `buildGraph` function has a nested loop that iterates over all pairs of words, which takes `O(n^2)` time. For each pair of words, it checks if the last character of one word matches the first character of the other word, which takes `O(w)` time. Therefore, the total time complexity of buildGraph is `O(n^2 * w)`. The `dfs` function performs a depth-first search of the graph, which takes `O(n + e)` time, where `e` is the number of edges in the graph. Since the graph can have at most `n^2` edges, the worst-case time complexity of dfs is `O(n^3)`. About the space complexity is `O(n^2)`. The `buildGraph` function creates a `Map` object with `n` keys and `n` `Set` objects, which takes `O(n^2)` space. The `dfs` function uses a set to keep track of visited vertices, which takes `O(n)` space. Therefore, the total space complexity is `O(n^2 + n)`, which simplifies to `O(n^2)`. Overall, the solution has a polynomial time complexity and space complexity in terms of the input size.