# Grind 75 034. Number of Islands

> Grid에서 land로 연결된 component 개수를 세는 문제다. 새 land를 만날 때마다 island 하나를 발견한 것이고, BFS/DFS로 그 island 전체를 지우면 된다.

## Link

- [LeetCode - Number of Islands](https://leetcode.com/problems/number-of-islands)

## Problem Model

`1`은 land, `0`은 water다. 상하좌우로 연결된 land들이 하나의 island를 이룬다. 대각선은 연결이 아니다.

## Pattern

- [Matrix](../../../01.%20Data%20Structures/04.%20Matrix.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
- [Graph Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/08.%20Graph%20Traversal%20Patterns.md)

## Approach

1. 모든 칸을 순회한다.
2. `1`을 발견하면 island count를 1 늘린다.
3. 해당 칸에서 DFS/BFS를 수행하며 연결된 모든 `1`을 `0`으로 바꾼다.
4. 이미 지운 land는 다시 count되지 않는다.

## Python 3.14 Solution

```python
class Solution:
    def numIslands(self, grid: list[list[str]]) -> int:
        rows, cols = len(grid), len(grid[0])
        islands = 0
        directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

        def sink(start_row: int, start_col: int) -> None:
            stack = [(start_row, start_col)]
            grid[start_row][start_col] = "0"

            while stack:
                row, col = stack.pop()
                for dr, dc in directions:
                    nr, nc = row + dr, col + dc
                    if not (0 <= nr < rows and 0 <= nc < cols):
                        continue
                    if grid[nr][nc] != "1":
                        continue
                    grid[nr][nc] = "0"
                    stack.append((nr, nc))

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == "1":
                    islands += 1
                    sink(row, col)

        return islands
```

## Correctness Notes

- 처음 만나는 `1`은 아직 방문하지 않은 새로운 island에 속한다.
- `sink`는 그 island와 상하좌우로 연결된 모든 land를 방문해 water로 바꾼다.
- 같은 island의 다른 칸은 이후 순회에서 다시 `1`로 발견되지 않으므로 중복 count가 없다.
- 모든 칸을 확인하므로 모든 island가 한 번씩 세어진다.

## Complexity

- Time: `O(mn)`
- Space: `O(mn)` in the worst case for the stack.

## Mistake Notes

- grid 값은 문자열 `"1"`, `"0"`이다. 정수 `1`, `0`과 혼동하지 말자.
- 방문 표시를 stack에 넣을 때 해야 중복 push를 줄일 수 있다.
- 대각선 방향은 포함하지 않는다.