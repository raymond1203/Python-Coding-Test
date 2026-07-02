# Grind 75 053. Partition Equal Subset Sum

> 배열을 합이 같은 두 subset으로 나눌 수 있는지 묻는 문제다. 전체 합이 짝수라면 target은 절반이고, target sum을 만들 수 있는지 확인하면 된다.

## Link

- [LeetCode - Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum)

## Problem Model

두 subset의 합이 같으려면 전체 합은 짝수여야 한다. 한 subset이 `total / 2`를 만들 수 있으면 나머지도 자동으로 같은 합이 된다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [Knapsack Style DP](../../../03.%20Problem%20Solving%20Patterns/23.%20Knapsack%20Style%20DP.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)

## Approach

0/1 knapsack의 가능 여부 DP로 푼다.

1. 전체 합이 홀수면 false다.
2. `target = total // 2`를 만든다.
3. `dp[x]`를 합 `x`를 만들 수 있는지로 둔다.
4. 각 숫자는 한 번만 쓸 수 있으므로 target에서 num까지 역순으로 갱신한다.

## Python 3.14 Solution

```python
class Solution:
    def canPartition(self, nums: list[int]) -> bool:
        total = sum(nums)
        if total % 2 == 1:
            return False

        target = total // 2
        dp = [False] * (target + 1)
        dp[0] = True

        for num in nums:
            for value in range(target, num - 1, -1):
                dp[value] = dp[value] or dp[value - num]

        return dp[target]
```

## Correctness Notes

- `dp[value]`는 지금까지 본 숫자들로 합 `value`를 만들 수 있는지를 나타낸다.
- 숫자 `num`을 사용해 `value`를 만들려면 이전 상태에서 `value - num`을 만들 수 있어야 한다.
- 역순 갱신은 같은 숫자가 같은 round에서 여러 번 사용되는 것을 막는다.
- target을 만들 수 있으면 나머지 합도 target이므로 equal partition이 가능하다.

## Complexity

- Time: `O(n * target)`
- Space: `O(target)`

## Mistake Notes

- 정방향으로 갱신하면 같은 숫자를 여러 번 사용하는 unbounded knapsack이 되어버린다.
- 전체 합이 홀수인 경우를 먼저 제거해야 한다.
- subset의 실제 원소가 아니라 가능 여부만 필요하므로 boolean DP면 충분하다.