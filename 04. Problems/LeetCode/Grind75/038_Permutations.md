# Grind 75 038. Permutations

> 모든 순열을 생성하는 backtracking 문제다. 현재 path에 아직 사용하지 않은 숫자를 하나씩 붙여가며 길이가 `n`이 되면 정답으로 기록한다.

## Link

- [LeetCode - Permutations](https://leetcode.com/problems/permutations)

## Problem Model

서로 다른 숫자들이 주어졌을 때 가능한 모든 ordering을 반환한다. 각 숫자는 순열 안에서 정확히 한 번만 사용된다.

## Pattern

- [Backtracking](../../../02.%20Algorithms/05.%20Backtracking.md)
- [Backtracking Search Patterns](../../../03.%20Problem%20Solving%20Patterns/09.%20Backtracking%20Search%20Patterns.md)
- [Recursion](../../../02.%20Algorithms/03.%20Recursion.md)

## Approach

1. `used[i]`로 `nums[i]`가 현재 path에 들어갔는지 표시한다.
2. path 길이가 `len(nums)`가 되면 하나의 순열이 완성된다.
3. 아직 사용하지 않은 모든 숫자를 선택해 재귀 호출한다.
4. 재귀가 끝나면 선택을 되돌린다.

## Python 3.14 Solution

```python
class Solution:
    def permute(self, nums: list[int]) -> list[list[int]]:
        result: list[list[int]] = []
        path: list[int] = []
        used = [False] * len(nums)

        def backtrack() -> None:
            if len(path) == len(nums):
                result.append(path.copy())
                return

            for i, num in enumerate(nums):
                if used[i]:
                    continue

                used[i] = True
                path.append(num)
                backtrack()
                path.pop()
                used[i] = False

        backtrack()
        return result
```

## Correctness Notes

- `used`는 각 숫자가 현재 순열에 최대 한 번만 들어가도록 보장한다.
- 각 depth에서 아직 사용하지 않은 모든 숫자를 시도하므로 가능한 모든 다음 선택을 탐색한다.
- 길이가 `n`인 path는 모든 숫자를 정확히 한 번 사용한 순열이다.
- 선택 후 되돌리기를 수행하므로 다른 분기 탐색에 상태가 섞이지 않는다.

## Complexity

- Time: `O(n * n!)`, result copy cost included.
- Space: `O(n)`, excluding output.

## Mistake Notes

- `path`를 그대로 append하면 이후 backtracking 변화가 결과에 반영된다. 반드시 copy한다.
- 입력 숫자가 distinct라는 조건 덕분에 중복 제거 로직이 필요 없다.
- 순열은 조합과 달리 start index를 쓰면 안 된다. 매 depth에서 모든 미사용 숫자가 후보가 된다.