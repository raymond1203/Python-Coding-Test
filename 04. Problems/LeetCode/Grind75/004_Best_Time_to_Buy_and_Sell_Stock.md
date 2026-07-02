# Grind 75 004. Best Time to Buy and Sell Stock

> 현재까지의 최저 매수가와 오늘 팔았을 때의 이익을 동시에 갱신하는 one-pass 문제다.

## Link

- [LeetCode - Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)

## Problem Model

한 번 사고 한 번 팔 수 있으며, 매수는 매도보다 먼저 일어나야 한다. 따라서 각 날짜를 매도 날짜로 볼 때, 그 이전의 최소 가격만 알면 최선의 이익을 계산할 수 있다.

## Pattern

- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)
- [Sort Then Scan](../../../03.%20Problem%20Solving%20Patterns/20.%20Sort%20Then%20Scan.md)

## Approach

1. `min_price`를 지금까지 본 가장 낮은 가격으로 유지한다.
2. 오늘 가격으로 팔면 `price - min_price`가 이익이다.
3. `best`를 최대 이익으로 갱신한다.
4. 오늘 가격이 더 낮으면 이후를 위해 `min_price`를 갱신한다.

## Python 3.14 Solution

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        min_price = float("inf")
        best = 0

        for price in prices:
            best = max(best, price - min_price)
            min_price = min(min_price, price)

        return best
```

## Complexity

- Time: O(n)
- Space: O(1)

## Mistake Points

- 가격을 정렬하면 날짜 순서가 깨지므로 정렬하면 안 된다.
- 이익이 음수라면 거래하지 않는 선택이 가능하므로 답은 0 이상이다.

## Related Notes

- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)
- [Greedy](../../../02.%20Algorithms/07.%20Greedy.md)
