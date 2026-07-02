# Grind 75 037. Combination Sum

> 후보 숫자를 여러 번 사용할 수 있고 합이 target이 되는 조합을 찾는 문제다. 선택 순서가 다른 같은 조합을 중복으로 세지 않으려면 start index를 유지해야 한다.

## Link

- [LeetCode - Combination Sum](https://leetcode.com/problems/combination-sum)

## Problem Model

각 candidate는 무제한 사용할 수 있다. 결과는 조합이므로 `[2, 2, 3]`과 `[2, 3, 2]`는 같은 답으로 보아야 한다.

## Pattern

- [Backtracking](../../../02.%20Algorithms/05.%20Backtracking.md)
- [Backtracking Search Patterns](../../../03.%20Problem%20Solving%20Patterns/09.%20Backtracking%20Search%20Patterns.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

1. 후보를 정렬한다.
2. `start` 이후 후보만 선택하도록 하여 조합 순서를 고정한다.
3. 같은 후보를 다시 사용할 수 있으므로 재귀 호출 시 `i`를 그대로 넘긴다.
4. 남은 합이 0이면 현재 path를 정답에 추가한다.
5. 후보가 남은 합보다 크면 이후 후보도 크므로 중단한다.

## Python 3.14 Solution

```python
class Solution:
    def combinationSum(self, candidates: list[int], target: int) -> list[list[int]]:
        candidates.sort()
        result: list[list[int]] = []
        path: list[int] = []

        def backtrack(start: int, remaining: int) -> None:
            if remaining == 0:
                result.append(path.copy())
                return

            for i in range(start, len(candidates)):
                candidate = candidates[i]
                if candidate > remaining:
                    break

                path.append(candidate)
                backtrack(i, remaining - candidate)
                path.pop()

        backtrack(0, target)
        return result
```

## Correctness Notes

- `start`를 유지하므로 path의 후보 index는 감소하지 않는다. 따라서 같은 조합의 순열들이 중복 생성되지 않는다.
- 재귀 호출에 `i`를 넘기므로 현재 후보를 다시 사용할 수 있다.
- 남은 합이 0이 되는 순간 path의 합은 target이므로 정답이다.
- 정렬 후 `candidate > remaining`이면 뒤 후보들도 사용할 수 없으므로 가지치기가 안전하다.

## Complexity

- Time: output size에 비례하는 exponential search.
- Space: `O(target / min(candidates))` recursion depth, excluding output.

## Mistake Notes

- `backtrack(i + 1, ...)`를 쓰면 같은 숫자를 여러 번 사용할 수 없게 된다.
- path를 결과에 넣을 때는 `path.copy()`가 필요하다.
- 조합 문제에서는 순서를 고정하는 장치가 중복 제거의 핵심이다.