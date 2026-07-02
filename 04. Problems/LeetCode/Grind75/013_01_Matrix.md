# Grind 75 013. 01 Matrix

> 모든 1에서 가장 가까운 0까지의 거리를 각각 구하는 문제다. 각 1에서 따로 BFS를 시작하면 느리므로, 모든 0을 동시에 시작점으로 넣는 multi-source BFS로 풀어야 한다.

## Link

- [LeetCode - 01 Matrix](https://leetcode.com/problems/01-matrix)

## Problem Model

각 칸을 정점, 상하좌우 인접을 간선으로 보는 unweighted graph다. 모든 칸에서 가장 가까운 0까지의 최단 거리를 구한다.

## Pattern

- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)
- [Graph Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/08.%20Graph%20Traversal%20Patterns.md)

## Approach

1. 거리 배열 `dist`를 `-1`로 초기화한다.
2. 값이 0인 모든 칸을 큐에 넣고 거리 0으로 둔다.
3. BFS를 수행한다.
4. 아직 거리 값이 없는 이웃 칸을 발견하면 현재 거리 + 1을 기록한다.
5. BFS가 끝나면 각 칸에는 가장 가까운 0까지의 거리가 들어 있다.

## Python 3.14 Solution

```python
from collections import deque


class Solution:
    def updateMatrix(self, mat: list[list[int]]) -> list[list[int]]:
        rows, cols = len(mat), len(mat[0])
        dist = [[-1] * cols for _ in range(rows)]
        queue: deque[tuple[int, int]] = deque()

        for row in range(rows):
            for col in range(cols):
                if mat[row][col] == 0:
                    dist[row][col] = 0
                    queue.append((row, col))

        directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

        while queue:
            row, col = queue.popleft()

            for dr, dc in directions:
                nr, nc = row + dr, col + dc
                if not (0 <= nr < rows and 0 <= nc < cols):
                    continue
                if dist[nr][nc] != -1:
                    continue

                dist[nr][nc] = dist[row][col] + 1
                queue.append((nr, nc))

        return dist
```

## Correctness Notes

- 모든 0을 거리 0의 시작점으로 동시에 넣으면 BFS layer가 “0으로부터의 거리”를 의미한다.
- unweighted graph에서 BFS는 처음 방문한 순간의 거리가 최단 거리다.
- `dist != -1`인 칸을 다시 방문하지 않으므로, 더 긴 경로로 갱신되는 일이 없다.

## Complexity

- Time: `O(mn)`
- Space: `O(mn)`

## Mistake Notes

- 각 1마다 BFS를 새로 돌리면 최악의 경우 `O((mn)^2)`가 된다.
- 이 문제는 한 출발점에서 여러 도착점을 찾는 문제가 아니라, 여러 출발점에서 전체 칸으로 퍼지는 문제다.
- `mat` 자체를 거리 배열로 덮어쓸 수도 있지만, `dist = -1` 방식이 방문 여부와 거리를 명확히 분리한다.

## Related Notes

- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
