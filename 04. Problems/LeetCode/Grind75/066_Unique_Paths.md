# Grind 75 066. Unique Paths

> 오른쪽 또는 아래로만 이동해 grid의 끝까지 가는 경로 수를 구하는 DP 문제다. 각 칸에 도달하는 방법 수는 위에서 오는 방법 수와 왼쪽에서 오는 방법 수의 합이다.

## Link

- [LeetCode - Unique Paths](https://leetcode.com/problems/unique-paths)

## Problem Model

`m x n` grid에서 시작점은 왼쪽 위, 도착점은 오른쪽 아래다. 이동 방향은 right/down으로 제한된다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)
- [Matrix](../../../01.%20Data%20Structures/04.%20Matrix.md)

## Approach

1. 첫 행과 첫 열은 경로가 하나뿐이다.
2. 1차원 DP 배열 `dp[col]`을 사용한다.
3. 각 칸에서 `dp[col]`은 위에서 오는 경로 수, `dp[col - 1]`은 왼쪽에서 오는 경로 수다.
4. `dp[col] += dp[col - 1]`로 갱신한다.

## Python 3.14 Solution

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [1] * n

        for _ in range(1, m):
            for col in range(1, n):
                dp[col] += dp[col - 1]

        return dp[-1]
```

## Correctness Notes

- 첫 행의 모든 칸은 오른쪽으로만 이동해 도달하므로 경로 수가 1이다.
- 새 행을 처리할 때 `dp[col]`는 위쪽 칸의 경로 수를 보존하고 있다.
- `dp[col - 1]`는 같은 행의 왼쪽 칸 경로 수로 이미 갱신되어 있다.
- 두 값을 더하면 현재 칸의 모든 도달 경로 수가 된다.

## Complexity

- Time: `O(mn)`
- Space: `O(n)`

## Mistake Notes

- combinatorics로도 풀 수 있지만 DP가 grid path 패턴을 이해하기 좋다.
- 첫 행/열의 base case를 1로 두는 이유를 명확히 해야 한다.