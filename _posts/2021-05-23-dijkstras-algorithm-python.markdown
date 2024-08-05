---
title: "Dijkstra's algorithm in Python"
categories: [Algorithms, Python]
tags: [Python, Graph Theory, Dijkstra's Algorithm]
date: 2021-05-23 11:55:00 -0500
math: true
mermaid: true
pin: false
media_subpath: /assets/img/posts/dijkstras-algorithm-python/
image: python-dijkstra-post.webp
---
If you're studying engineering or computer science, you've likely encountered graph theory, a fascinating field with myriad applications in these disciplines.

Dijkstra's algorithm is a fundamental concept in graph theory, designed to find the shortest path between nodes in a graph. This graph can represent anything from a network infrastructure to the travel cost between cities using specific routes. Crucially, this algorithm relies on tags with positive numbers, representing the cost associated with traversing an edge.

The essence of Dijkstra's algorithm lies in systematically exploring all the shortest paths from an origin node to all other nodes. The process stops when it identifies the shortest path to all nodes or reaches a specific target. However, it's essential to note that this algorithm is not suitable for graphs with negative edge costs. In such cases, you'd want to implement the Bellman-Ford algorithm.

## The Algorithm

Let's break down the steps of Dijkstra's algorithm for finding the shortest path:

1. Initialize a vector $$V$$ of size $$N$$ (number of nodes) to store distances from the initial node $$X$$ to all other nodes. Initialize distances on $$V$$ with the maximum possible value, except for the starting node $$X$$.
2. Set the current node to $$A \leftarrow X$$.
3. Iterate over all adjacent nodes of $$A$$, excluding those that have already been visited. In other words, consider unvisited nodes $$\left \{ P_{1}, P_{2}, P_{3}, ..., P_{N} \right \}$$.
4. Compute the potential distance from $$A$$ to its neighbors using the formula: $$\mathrm{dp(} P_{i} \mathrm{)} = V_{A} + \mathrm{d(}A,V_{i}\mathrm{)}$$, where $$\mathrm{d(}A,V_{i}\mathrm{)}$$ represents the distance from $$A$$ to $$P_{i}$$. If this computed distance is shorter than the currently stored distance in $$V$$, update $$V$$ with this shorter distance.
5. Mark node $$A$$ as visited.
6. Set the next current node $$A$$ to be the one with the smallest distance in $$V$$ and repeat the process from step 3 until there are no unvisited nodes left.

## Python Implementation

In Python, you can implement Dijkstra's algorithm with an adjacency matrix representing the weighted graph, the initial node, and optionally, the target node. Below is a Python code snippet that follows the algorithm's steps:

```python
def find_all(wmat, start, end=-1):
    n = len(wmat)

    dist = [inf]*n
    dist[start] = wmat[start][start]  # 0

    spVertex = [False]*n
    parent = [-1]*n

    path = [{}]*n

    for count in range(n-1):
        minix = inf
        u = 0

        for v in range(len(spVertex)):
            if spVertex[v] == False and dist[v] <= minix:
                minix = dist[v]
                u = v

        spVertex[u] = True
        for v in range(n):
            if not(spVertex[v]) and wmat[u][v] != 0 and dist[u] + wmat[u][v] < dist[v]:
                parent[v] = u
                dist[v] = dist[u] + wmat[u][v]

    for i in range(n):
        j = i
        s = []
        while parent[j] != -1:
            s.append(j)
            j = parent[j]
        s.append(start)
        path[i] = s[::-1]

    return (dist[end], path[end]) if end >= 0 else (dist, path)
```

To see if the algorithm works correctly, let's create an example of an adjacency matrix for a weighted graph:

```mermaid
---
title: Sample graph
---
graph LR
    A((a))
    A -- 2 --- B((b))
    A -- 1 --- F((f))
    B -- 2 --- C((c))
    B -- 2 --- D((d))
    B -- 4 --- E((e))
    C -- 3 --- E((e))
    C -- 1 --- Z((z))
    D -- 4 --- E((e))
    D -- 3 --- F((f))
    E -- 7 --- G((g))
    F -- 5 --- G((g))
    G -- 6 --- Z((z))
```

```python
wmat = [[0, 2, 0, 0, 0, 1, 0, 0],
        [2, 0, 2, 2, 4, 0, 0, 0],
        [0, 2, 0, 0, 3, 0, 0, 1],
        [0, 2, 0, 0, 4, 3, 0, 0],
        [0, 4, 3, 4, 0, 0, 7, 0],
        [1, 0, 0, 3, 0, 0, 5, 0],
        [0, 0, 0, 0, 7, 5, 0, 6],
        [0, 0, 1, 0, 0, 0, 6, 0]]
```

## Python Module

You can download and import the Python module for Dijkstra's algorithm [here](https://github.com/crixodia/python-dijkstra) and import it.

```python
import dijkstra
```

## Find Distances and Paths

### Find All Distances and Paths

To find all distances and paths from the initial node to all other nodes, use the following function:

```python
dijkstra.find_all(wmat, start, end=-1):
```

It returns a tuple containing distances and paths lists. For example:

```python
print(dijkstra.find_all(wmat, 0))
```

**Output:**

```bash
(
    [0, 2, 4, 4, 6, 1, 6, 5],
    [
        [0],
        [0, 1],
        [0, 1, 2],
        [0, 5, 3],
        [0, 1, 4],
        [0, 5],
        [0, 5, 6],
        [0, 1, 2, 7]
    ]
)
```

### Find the Shortest Path

To find the shortest path from the initial node to the target node, use this function:

```python
dijkstra.find_shortest_path(wmat, start, end=-1)
```

For example, finding the shortest path from node 0 to node 7:

```python
print(dijkstra.find_shortest_path(wmat, 0, 7))
```

**Output:**

```bash
[0, 1, 2, 7]
```

If you want the path without specifying the target node:

```python
print(dijkstra.find_shortest_path(wmat, 0))
```

**Output:**

```bash
[[0], [0, 1], [0, 1, 2], [0, 5, 3], [0, 1, 4], [0, 5], [0, 5, 6], [0, 1, 2, 7]]
```

### Find the Shortest Distance

To find the shortest distance from the initial node to the target node, use this function:

```python
dijkstra.find_shortest_distance(wmat, start, end=-1)
```

For example, finding the shortest distance from node 0 to node 7:

```python
print(dijkstra.find_shortest_distance(wmat, 0, 7))
```

**Output:**

```bash
5
```

If you want the distance without specifying the target node:

```python
print(dijkstra.find_shortest_distance(wmat, 0))
```

**Output:**

```bash
[0, 2, 4, 4, 6, 1, 6, 5]
```

You can download the Python module [here](https://github.com/crixodia/python-dijkstra). Feel free to leave your comments and suggestions.
