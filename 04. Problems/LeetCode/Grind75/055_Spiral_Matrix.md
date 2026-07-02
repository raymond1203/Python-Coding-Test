# Grind 75 055. Spiral Matrix

> Matrix를 바깥 테두리부터 시계 방향으로 읽는 문제다. 네 개의 boundary를 유지하고 한 바퀴를 돌 때마다 안쪽으로 좁히면 된다.

## Link

- [LeetCode - Spiral Matrix](https://leetcode.com/problems/spiral-matrix)

## Problem Model

`top`, `bottom`, `left`, `right`가 아직 읽지 않은 직사각형 영역의 경계다. 위쪽 행, 오른쪽 열, 아래쪽 행, 왼쪽 열 순서로 읽고 경계를 줄인다.

## Pattern

- [Matrix](../../../01.%20Data%20Structures/04.%20Matrix.md)
- [Matrix Traversal](../../../03.%20Problem%20Solving%20Patterns/12.%20Matrix%20Traversal.md)

## Approach

1. 위쪽 행을 왼쪽에서 오른쪽으로 읽고 `top`을 내린다.
2. 오른쪽 열을 위에서 아래로 읽고 `right`를 줄인다.
3. 아직 행이 남아 있으면 아래쪽 행을 오른쪽에서 왼쪽으로 읽고 `bottom`을 올린다.
4. 아직 열이 남아 있으면 왼쪽 열을 아래에서 위로 읽고 `left`를 올린다.

## Python 3.14 Solution

```python
class Solution:
    def spiralOrder(self, matrix: list[list[int]]) -> list[int]:
        result: list[int] = []
        top, bottom = 0, len(matrix) - 1
        left, right = 0, len(matrix[0]) - 1

        while top <= bottom and left <= right:
            for col in range(left, right + 1):
                result.append(matrix[top][col])
            top += 1

            for row in range(top, bottom + 1):
                result.append(matrix[row][right])
            right -= 1

            if top <= bottom:
                for col in range(right, left - 1, -1):
                    result.append(matrix[bottom][col])
                bottom -= 1

            if left <= right:
                for row in range(bottom, top - 1, -1):
                    result.append(matrix[row][left])
                left += 1

        return result
```

## Correctness Notes

- 각 반복은 현재 남은 직사각형의 바깥 테두리를 시계 방향으로 정확히 한 번 읽는다.
- 한 방향을 읽은 뒤 해당 boundary를 줄이므로 같은 칸을 다시 읽지 않는다.
- 단일 행 또는 단일 열만 남은 경우를 조건문으로 확인해 중복 읽기를 방지한다.
- 경계가 교차하면 모든 칸을 읽은 것이므로 종료한다.

## Complexity

- Time: `O(mn)`
- Space: `O(1)` auxiliary space, excluding output.

## Mistake Notes

- 아래쪽 행과 왼쪽 열을 읽기 전에는 아직 유효한 행/열이 남았는지 확인해야 한다.
- rectangular matrix도 처리해야 하므로 정사각형이라고 가정하면 안 된다.
- 방문 set 없이 boundary만으로 충분하다.