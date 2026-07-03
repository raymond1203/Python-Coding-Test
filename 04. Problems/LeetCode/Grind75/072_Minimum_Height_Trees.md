# Grind 75 072. Minimum Height Trees

> Tree에서 root를 어디로 잡아야 높이가 최소가 되는지 찾는 문제다. Tree의 center를 찾는 문제로 볼 수 있고, leaf를 바깥에서 안쪽으로 벗겨내면 마지막에 center만 남는다.

## Link

- [LeetCode - Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees)

## Problem Model

Undirected tree의 root를 선택하면 height가 결정된다. 최소 height root는 tree의 중심이며, 중심은 하나 또는 두 개다.

## Pattern

- [Graph](../../../01.%20Data%20Structures/09.%20Graph.md)
- [Topological Sort](../../../02.%20Algorithms/10.%20Topological%20Sort.md)
- [Graph Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/08.%20Graph%20Traversal%20Patterns.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)

## Approach

1. 각 node의 degree와 adjacency list를 만든다.
2. degree가 1인 leaf들을 queue에 넣는다.
3. leaf layer를 제거하며 남은 node 수를 줄인다.
4. 새롭게 degree가 1이 된 node를 다음 leaf로 넣는다.
5. 남은 node 수가 2 이하가 되면 그 node들이 center다.

## Python 3.14 Solution

```python
from collections import deque


class Solution:
    def findMinHeightTrees(self, n: int, edges: list[list[int]]) -> list[int]:
        if n == 1:
            return [0]

        graph = [set() for _ in range(n)]
        for a, b in edges:
            graph[a].add(b)
            graph[b].add(a)

        leaves: deque[int] = deque(node for node in range(n) if len(graph[node]) == 1)
        remaining = n

        while remaining > 2:
            layer_size = len(leaves)
            remaining -= layer_size

            for _ in range(layer_size):
                leaf = leaves.popleft()
                neighbor = graph[leaf].pop()
                graph[neighbor].remove(leaf)

                if len(graph[neighbor]) == 1:
                    leaves.append(neighbor)

        return list(leaves)
```

## Correctness Notes

- Tree의 최외곽 leaf들은 어떤 최소 높이 root도 될 수 없다. leaf를 root로 잡으면 그 neighbor를 root로 잡는 것보다 높이가 작아질 수 없다.
- leaf layer를 동시에 제거하면 tree의 중심 방향으로 한 단계씩 줄어든다.
- Tree center는 diameter의 가운데에 해당하며 하나 또는 두 개다.
- 남은 node가 2개 이하일 때 이들이 가능한 minimum height root다.

## Complexity

- Time: `O(n)`
- Space: `O(n)`

## Mistake Notes

- leaf를 하나씩 제거하면 layer 개념이 깨질 수 있다. 현재 layer 크기를 고정해야 한다.
- `n == 1`은 edge가 없고 degree 0이므로 별도 처리한다.
- 일반 topological sort는 directed graph지만, 여기서는 undirected tree를 leaf pruning하는 유사 패턴이다.