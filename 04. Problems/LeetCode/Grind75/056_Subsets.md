# Grind 75 056. Subsets

> 모든 subset을 생성하는 문제다. 각 원소에 대해 선택하거나 선택하지 않는 decision tree를 만들면 전체 power set을 얻을 수 있다.

## Link

- [LeetCode - Subsets](https://leetcode.com/problems/subsets)

## Problem Model

서로 다른 숫자들의 모든 부분집합을 반환한다. 원소 수가 `n`이면 subset 수는 `2^n`개다.

## Pattern

- [Backtracking](../../../02.%20Algorithms/05.%20Backtracking.md)
- [Backtracking Search Patterns](../../../03.%20Problem%20Solving%20Patterns/09.%20Backtracking%20Search%20Patterns.md)
- [Recursion](../../../02.%20Algorithms/03.%20Recursion.md)

## Approach

1. 현재 path를 결과에 먼저 추가한다.
2. `start` 이후 원소를 하나씩 선택한다.
3. 선택한 원소를 path에 넣고 다음 index부터 재귀 호출한다.
4. 재귀 후 선택을 되돌린다.

## Python 3.14 Solution

```python
class Solution:
    def subsets(self, nums: list[int]) -> list[list[int]]:
        result: list[list[int]] = []
        path: list[int] = []

        def backtrack(start: int) -> None:
            result.append(path.copy())

            for i in range(start, len(nums)):
                path.append(nums[i])
                backtrack(i + 1)
                path.pop()

        backtrack(0)
        return result
```

## Correctness Notes

- `result.append(path.copy())`는 현재까지 선택한 원소로 구성된 subset을 기록한다.
- `start` 이후 원소만 선택하므로 같은 subset이 다른 순서로 중복 생성되지 않는다.
- 각 원소는 선택되거나 선택되지 않는 모든 가능성을 재귀 분기가 포괄한다.
- 모든 depth에서 현재 path를 기록하므로 크기 0부터 n까지 모든 subset이 포함된다.

## Complexity

- Time: `O(n * 2^n)`, output copy cost included.
- Space: `O(n)`, excluding output.

## Mistake Notes

- 빈 subset도 정답에 포함되어야 한다.
- path를 그대로 넣지 말고 copy해서 넣어야 한다.
- 순열이 아니므로 매 단계에서 전체 원소를 다시 후보로 쓰면 중복이 생긴다.