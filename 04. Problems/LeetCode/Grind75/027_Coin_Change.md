# Grind 75 027. Coin Change

> 금액을 만드는 최소 동전 수를 구하는 unbounded knapsack 계열 DP 문제다. 같은 동전을 여러 번 사용할 수 있으므로 amount를 작은 값부터 채운다.

## Link

- [LeetCode - Coin Change](https://leetcode.com/problems/coin-change)

## Problem Model

`dp[x]`를 amount `x`를 만들기 위한 최소 동전 수로 둔다. 어떤 coin을 마지막에 사용했다면 직전 상태는 `x - coin`이다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)
- [Knapsack Style DP](../../../03.%20Problem%20Solving%20Patterns/23.%20Knapsack%20Style%20DP.md)

## Approach

1. `dp[0] = 0`으로 시작한다.
2. 나머지는 불가능을 뜻하는 큰 값으로 초기화한다.
3. `total`을 1부터 amount까지 늘리며 모든 coin을 시도한다.
4. `coin <= total`이면 `dp[total - coin] + 1`로 갱신한다.
5. 최종값이 여전히 큰 값이면 불가능이다.

## Python 3.14 Solution

```python
class Solution:
    def coinChange(self, coins: list[int], amount: int) -> int:
        impossible = amount + 1
        dp = [impossible] * (amount + 1)
        dp[0] = 0

        for total in range(1, amount + 1):
            for coin in coins:
                if coin <= total:
                    dp[total] = min(dp[total], dp[total - coin] + 1)

        return -1 if dp[amount] == impossible else dp[amount]
```

## Correctness Notes

- `dp[total]`은 마지막 동전으로 어떤 coin을 썼는지에 따라 `dp[total - coin] + 1` 중 최솟값이다.
- `total`을 작은 값부터 계산하므로 `dp[total - coin]`은 이미 최적으로 계산되어 있다.
- 모든 coin을 시도하므로 가능한 모든 마지막 선택을 고려한다.
- sentinel 값이 남아 있으면 어떤 조합으로도 만들 수 없다는 뜻이다.

## Complexity

- Time: `O(amount * len(coins))`
- Space: `O(amount)`

## Mistake Notes

- Greedy로 가장 큰 동전부터 고르는 방식은 일반 coin set에서 틀릴 수 있다.
- `dp[0] = 0`이 없으면 모든 상태가 갱신되지 않는다.
- 불가능 sentinel은 `amount + 1`이면 충분하다. 최소 동전 수가 amount보다 클 수 없기 때문이다.