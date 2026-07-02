# Grind 75 010. Maximum Subarray

> 각 위치에서 “여기서 끝나는 최대 부분배열 합”만 알면 전체 최댓값을 갱신할 수 있다. Kadane's Algorithm의 핵심은 손해가 되는 과거 합을 과감히 버리는 것이다.

## Link

- [LeetCode - Maximum Subarray](https://leetcode.com/problems/maximum-subarray)

## Problem Model

연속된 subarray의 합 중 최댓값을 구한다. 비연속 subsequence가 아니라 반드시 contiguous subarray라는 점이 중요하다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

`current`를 “현재 원소에서 끝나는 최대 subarray sum”으로 둔다.

- 이전 subarray에 현재 값을 붙이는 편이 좋으면 `current + num`
- 새로 시작하는 편이 좋으면 `num`

둘 중 큰 값을 새 `current`로 선택하고, 매번 `best`를 갱신한다.

## Python 3.14 Solution

```python
class Solution:
    def maxSubArray(self, nums: list[int]) -> int:
        current = best = nums[0]

        for num in nums[1:]:
            current = max(num, current + num)
            best = max(best, current)

        return best
```

## Correctness Notes

- 어떤 최적 subarray도 특정 index에서 끝난다.
- index `i`에서 끝나는 최적 subarray는 `nums[i]` 하나로 시작하거나, `i - 1`에서 끝나는 최적 subarray에 `nums[i]`를 붙인 것 중 하나다.
- `current`가 이 값을 항상 보존하므로, `best`는 지금까지 본 모든 끝점의 최댓값이 된다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- 모든 값이 음수일 수 있으므로 `best = 0`으로 시작하면 틀린다.
- subarray는 연속이어야 한다. 음수 하나를 임의로 건너뛰는 식의 greedy는 잘못된 모델이다.
- `current`는 “지금까지의 최대”가 아니라 “현재 위치에서 끝나는 최대”다.

## Related Notes

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)
