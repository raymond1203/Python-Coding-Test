# Grind 75 063. Largest Rectangle in Histogram

> 각 막대를 높이로 하는 최대 직사각형의 너비를 찾는 문제다. 현재 막대보다 낮은 막대가 나타나는 순간, 이전 높은 막대들의 오른쪽 경계가 확정된다.

## Link

- [LeetCode - Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram)

## Problem Model

각 bar를 직사각형의 최소 높이로 삼을 때, 왼쪽과 오른쪽으로 자신보다 낮은 첫 bar 사이가 가능한 최대 너비다. 이를 monotonic increasing stack으로 찾는다.

## Pattern

- [Monotonic Stack](../../../03.%20Problem%20Solving%20Patterns/06.%20Monotonic%20Stack.md)
- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)

## Approach

1. stack에는 높이가 증가하는 bar의 index를 저장한다.
2. 현재 높이가 stack top 높이보다 낮으면 top bar의 오른쪽 경계가 현재 index로 확정된다.
3. pop한 bar의 왼쪽 경계는 pop 이후 stack top이다.
4. 마지막에 높이 0 sentinel을 추가해 남은 bar를 모두 처리한다.

## Python 3.14 Solution

```python
class Solution:
    def largestRectangleArea(self, heights: list[int]) -> int:
        stack: list[int] = []
        best = 0
        extended = heights + [0]

        for i, height in enumerate(extended):
            while stack and extended[stack[-1]] > height:
                h = extended[stack.pop()]
                left_boundary = stack[-1] if stack else -1
                width = i - left_boundary - 1
                best = max(best, h * width)

            stack.append(i)

        return best
```

## Correctness Notes

- stack은 index의 높이가 non-decreasing이 되도록 유지된다.
- 더 낮은 높이를 만나 pop되는 bar는 현재 index가 오른쪽 첫 낮은 bar다.
- pop 이후 stack top은 왼쪽 첫 낮은 bar이므로 그 사이가 pop된 bar 높이로 만들 수 있는 최대 너비다.
- 모든 bar가 pop되는 순간 자신의 최대 직사각형 후보를 계산하므로 최댓값을 얻는다.

## Complexity

- Time: `O(n)`
- Space: `O(n)`

## Mistake Notes

- sentinel 0을 추가하지 않으면 끝까지 증가하는 stack을 따로 처리해야 한다.
- width는 `i - left_boundary - 1`이다.
- 같은 높이는 pop하지 않고 유지해도 정답에는 문제가 없다.