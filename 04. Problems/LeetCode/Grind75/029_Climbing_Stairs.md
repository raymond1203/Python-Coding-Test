# Grind 75 029. Climbing Stairs

> 한 번에 1칸 또는 2칸을 오를 수 있을 때, `n`번째 계단에 도달하는 방법 수를 구하는 문제다. 마지막 이동이 1칸인지 2칸인지로 나누면 Fibonacci 형태의 DP가 된다.

## Link

- [LeetCode - Climbing Stairs](https://leetcode.com/problems/climbing-stairs)

## Problem Model

`ways[i]`를 `i`번째 계단에 도달하는 방법 수로 둔다. `i`에 도달하는 마지막 이동은 `i - 1`에서 1칸 오거나, `i - 2`에서 2칸 오르는 두 경우뿐이다.

## Pattern

- [Dynamic Programming](../../../02.%20Algorithms/06.%20Dynamic%20Programming.md)
- [DP State Design](../../../03.%20Problem%20Solving%20Patterns/22.%20DP%20State%20Design.md)

## Approach

1. `ways[1] = 1`, `ways[2] = 2`로 시작한다.
2. `ways[i] = ways[i - 1] + ways[i - 2]`를 사용한다.
3. 전체 배열이 필요하지 않으므로 직전 두 값만 보관한다.

## Python 3.14 Solution

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n

        two_before = 1
        one_before = 2

        for _ in range(3, n + 1):
            current = one_before + two_before
            two_before = one_before
            one_before = current

        return one_before
```

## Correctness Notes

- `i`번째 계단에 도착하는 모든 방법은 마지막 이동 기준으로 `i - 1`에서 오는 경우와 `i - 2`에서 오는 경우로 정확히 분리된다.
- 두 경우는 서로 겹치지 않으므로 더하면 전체 방법 수가 된다.
- 반복문은 작은 계단 수부터 차례로 정확한 값을 계산하므로 최종적으로 `n`의 방법 수를 얻는다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- `n = 1`, `n = 2` base case를 먼저 처리하면 구현이 단순하다.
- 조합 공식으로 접근할 수도 있지만, 코딩 테스트에서는 DP 점화식 설명이 더 안정적이다.