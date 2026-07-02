# Grind 75 009. Flood Fill

> 시작 칸과 같은 색으로 연결된 컴포넌트를 찾아 새 색으로 칠하는 문제다. 본질은 grid graph에서의 BFS/DFS traversal이다.

## Link

- [LeetCode - Flood Fill](https://leetcode.com/problems/flood-fill)

## Problem Model

이미지의 각 칸을 정점으로 보고, 상하좌우로 연결된 같은 색 칸만 이동할 수 있는 그래프로 해석한다. 시작점 `(sr, sc)`가 속한 connected component 전체를 `color`로 바꾼다.

## Pattern

- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
- [Matrix](../../../01.%20Data%20Structures/04.%20Matrix.md)
- [Graph](../../../01.%20Data%20Structures/09.%20Graph.md)

## Approach

1. 시작 색 `original`을 저장한다.
2. `original == color`면 이미 목표 상태이므로 바로 반환한다.
3. 시작 칸을 큐에 넣고 새 색으로 바꾼다.
4. 큐에서 칸을 꺼내 상하좌우를 확인한다.
5. 범위 안이고 아직 `original` 색이면 새 색으로 바꾸고 큐에 넣는다.

## Python 3.14 Solution

```python
from collections import deque


class Solution:
    def floodFill(
        self,
        image: list[list[int]],
        sr: int,
        sc: int,
        color: int,
    ) -> list[list[int]]:
        original = image[sr][sc]
        if original == color:
            return image

        rows, cols = len(image), len(image[0])
        queue: deque[tuple[int, int]] = deque([(sr, sc)])
        image[sr][sc] = color

        directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

        while queue:
            row, col = queue.popleft()

            for dr, dc in directions:
                nr, nc = row + dr, col + dc
                if not (0 <= nr < rows and 0 <= nc < cols):
                    continue
                if image[nr][nc] != original:
                    continue

                image[nr][nc] = color
                queue.append((nr, nc))

        return image
```

## Correctness Notes

- 큐에는 시작점과 같은 색으로 연결되어 있고 아직 처리할 필요가 있는 칸만 들어간다.
- 칸을 큐에 넣는 순간 색을 바꾸므로 같은 칸이 중복으로 큐에 들어가지 않는다.
- 상하좌우로 도달 가능한 모든 `original` 칸은 BFS 확장 과정에서 반드시 방문된다.
- 다른 색이거나 component 밖의 칸은 조건에서 제외되므로 바뀌지 않는다.

## Complexity

- Time: `O(mn)`
- Space: `O(mn)` in the worst case because the queue can contain many cells.

## Mistake Notes

- `original == color`인 경우를 처리하지 않으면 DFS/BFS가 같은 칸을 계속 다시 칠할 수 있다.
- 방문 처리는 큐에서 꺼낼 때가 아니라 큐에 넣을 때 하는 편이 중복을 줄인다.
- 대각선은 연결로 보지 않는다. 방향 배열은 반드시 네 방향이다.

## Related Notes

- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
