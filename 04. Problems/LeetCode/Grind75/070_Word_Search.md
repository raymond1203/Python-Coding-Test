# Grind 75 070. Word Search

> Grid에서 상하좌우로 이동하며 주어진 단어를 만들 수 있는지 확인하는 backtracking 문제다. 한 경로 안에서 같은 칸을 두 번 사용할 수 없으므로 방문 표시와 되돌리기가 핵심이다.

## Link

- [LeetCode - Word Search](https://leetcode.com/problems/word-search)

## Problem Model

각 칸의 문자를 하나씩 사용해 word를 순서대로 만들어야 한다. 이동은 상하좌우만 가능하고, 같은 칸은 한 번의 path에서 한 번만 사용할 수 있다.

## Pattern

- [Matrix](../../../01.%20Data%20Structures/04.%20Matrix.md)
- [Backtracking](../../../02.%20Algorithms/05.%20Backtracking.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)
- [Backtracking Search Patterns](../../../03.%20Problem%20Solving%20Patterns/09.%20Backtracking%20Search%20Patterns.md)

## Approach

1. 모든 칸을 시작점으로 시도한다.
2. 현재 칸이 word의 현재 문자와 다르면 실패한다.
3. 문자가 맞으면 임시로 방문 표시를 한다.
4. 다음 문자 index로 상하좌우를 재귀 탐색한다.
5. 탐색 후 원래 문자로 되돌린다.

## Python 3.14 Solution

```python
class Solution:
    def exist(self, board: list[list[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])
        directions = ((1, 0), (-1, 0), (0, 1), (0, -1))

        def backtrack(row: int, col: int, index: int) -> bool:
            if index == len(word):
                return True
            if not (0 <= row < rows and 0 <= col < cols):
                return False
            if board[row][col] != word[index]:
                return False

            original = board[row][col]
            board[row][col] = "#"

            for dr, dc in directions:
                if backtrack(row + dr, col + dc, index + 1):
                    board[row][col] = original
                    return True

            board[row][col] = original
            return False

        for row in range(rows):
            for col in range(cols):
                if backtrack(row, col, 0):
                    return True

        return False
```

## Correctness Notes

- `backtrack(row, col, index)`는 현재 위치에서 `word[index:]`를 만들 수 있는지 판단한다.
- 현재 문자가 맞을 때만 다음 index로 이동하므로 path의 문자 순서가 word와 일치한다.
- 방문 표시 `#`는 같은 path에서 같은 칸을 재사용하지 못하게 한다.
- 모든 시작점과 모든 가능한 방향 선택을 탐색하므로 가능한 경로를 놓치지 않는다.

## Complexity

- Time: `O(mn * 4^L)` upper bound, where `L = len(word)`.
- Space: `O(L)` recursion stack.

## Mistake Notes

- 재귀가 성공하더라도 반환 전에 board를 복구해야 한다.
- 대각선 이동은 허용되지 않는다.
- 방문 set을 써도 되지만 in-place marker가 공간을 줄인다.