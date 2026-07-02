# Grind 75 035. Rotting Oranges

> 썩은 오렌지가 매분 인접한 신선한 오렌지를 썩게 만드는 전파 문제다. 모든 초기 rotten orange를 동시에 시작점으로 넣는 multi-source BFS가 핵심이다.

## Link

- [LeetCode - Rotting Oranges](https://leetcode.com/problems/rotting-oranges)

## Problem Model

각 칸을 정점으로 보고, 상하좌우 인접을 간선으로 본다. rotten orange에서 fresh orange로 퍼지는 최소 시간을 구한다.

## Pattern

- [Matrix](../../../01.%20Data%20Structures/04.%20Matrix.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)

## Approach

1. 모든 rotten orange를 queue에 넣고 fresh orange 수를 센다.
2. BFS layer 하나가 1분을 의미한다.
3. fresh orange가 rotten 되면 fresh 수를 줄이고 queue에 넣는다.
4. BFS가 끝났을 때 fresh가 남아 있으면 `-1`, 아니면 걸린 시간을 반환한다.

## Python 3.14 Solution

```python
from collections import deque


class Solution:
    def orangesRotting(self, grid: list[list[int]]) -> int:
        rows, cols = len(grid), len(grid[0])
        queue: deque[tuple[int, int]] = deque()
        fresh = 0

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == 2:
                    queue.append((row, col))
                elif grid[row][col] == 1:
                    fresh += 1

        minutes = 0
        directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

        while queue and fresh > 0:
            for _ in range(len(queue)):
                row, col = queue.popleft()
                for dr, dc in directions:
                    nr, nc = row + dr, col + dc
                    if not (0 <= nr < rows and 0 <= nc < cols):
                        continue
                    if grid[nr][nc] != 1:
                        continue

                    grid[nr][nc] = 2
                    fresh -= 1
                    queue.append((nr, nc))

            minutes += 1

        return minutes if fresh == 0 else -1
```

## Correctness Notes

- 초기 queue는 시간 0에 이미 rotten인 모든 시작점을 담는다.
- BFS의 한 layer는 정확히 1분 동안 동시에 썩는 과정을 나타낸다.
- fresh orange는 처음 rotten 되는 순간이 가장 빠른 시간이다.
- BFS 후 fresh가 남으면 어떤 rotten source에서도 도달할 수 없는 orange가 있다는 뜻이다.

## Complexity

- Time: `O(mn)`
- Space: `O(mn)`

## Mistake Notes

- 초기 rotten orange를 하나씩 따로 BFS하면 시간이 왜곡된다. 반드시 동시에 시작해야 한다.
- fresh가 처음부터 0이면 `0`을 반환해야 한다.
- minutes를 layer 처리 후 증가시키는 구조에서는 `while queue and fresh > 0` 조건이 off-by-one을 막아준다.