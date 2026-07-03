# Grind 75 061. Maximum Profit in Job Scheduling

> 겹치지 않는 작업들을 골라 최대 profit을 만드는 weighted interval scheduling 문제다. 종료 시간 기준으로 정렬하고, 각 작업을 선택할 때 이전에 끝난 최적 profit을 binary search로 찾는다.

## Link

- [LeetCode - Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling)

## Problem Model

각 job은 start, end, profit을 가진다. 서로 겹치지 않는 job들을 골라 profit 합을 최대로 해야 한다. `end <= next_start`는 함께 선택 가능하다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Sweep Line and Intervals](../../../03.%20Problem%20Solving%20Patterns/19.%20Sweep%20Line%20and%20Intervals.md)

## Approach

1. job을 end time 기준으로 정렬한다.
2. `ends`에는 처리한 job들의 종료 시간을 저장한다.
3. `dp[i]`는 end time이 `ends[i - 1]` 이하인 job들로 얻을 수 있는 최대 profit이다.
4. 현재 job을 선택하는 profit은 `dp[compatible_index] + profit`이다.
5. 선택하지 않는 profit은 이전 최적값 `dp[-1]`이다.

## Python 3.14 Solution

```python
from bisect import bisect_right


class Solution:
    def jobScheduling(
        self,
        startTime: list[int],
        endTime: list[int],
        profit: list[int],
    ) -> int:
        jobs = sorted(zip(startTime, endTime, profit), key=lambda job: job[1])
        ends: list[int] = []
        dp = [0]

        for start, end, earn in jobs:
            compatible = bisect_right(ends, start)
            take = dp[compatible] + earn
            skip = dp[-1]

            ends.append(end)
            dp.append(max(skip, take))

        return dp[-1]
```

## Correctness Notes

- 종료 시간 기준 정렬로 현재 job보다 먼저 끝나는 job들의 최적값이 이미 계산되어 있다.
- `bisect_right(ends, start)`는 현재 job 시작 시각 이하로 끝난 job 수를 반환한다.
- 현재 job을 선택하는 최적해는 compatible prefix의 최적 profit에 현재 profit을 더한 값이다.
- 현재 job을 선택하지 않는 경우와 선택하는 경우 중 큰 값을 저장하므로 모든 job prefix의 최적값이 유지된다.

## Complexity

- Time: `O(n log n)`
- Space: `O(n)`

## Mistake Notes

- 시작 시간 정렬보다 종료 시간 정렬이 DP compatible prefix를 다루기 쉽다.
- `end <= start`인 job은 함께 선택 가능하므로 `bisect_right`를 사용한다.
- `dp`를 1-indexed처럼 두면 compatible job 수를 그대로 index로 쓸 수 있어 편하다.