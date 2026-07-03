# Grind 75 068. Container With Most Water

> 두 vertical line으로 만들 수 있는 최대 물통 면적을 구하는 문제다. 면적은 더 낮은 높이에 의해 제한되므로, 낮은 쪽 포인터를 움직여야 개선 가능성이 생긴다.

## Link

- [LeetCode - Container With Most Water](https://leetcode.com/problems/container-with-most-water)

## Problem Model

두 index `left`, `right`를 고르면 면적은 `(right - left) * min(height[left], height[right])`다. 너비를 줄이면서도 높이 개선 가능성을 찾아야 한다.

## Pattern

- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

1. 양끝에서 시작한다.
2. 현재 면적을 계산해 best를 갱신한다.
3. 더 낮은 쪽 포인터를 안쪽으로 이동한다.
4. 두 포인터가 만날 때까지 반복한다.

## Python 3.14 Solution

```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        left, right = 0, len(height) - 1
        best = 0

        while left < right:
            width = right - left
            current_height = min(height[left], height[right])
            best = max(best, width * current_height)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return best
```

## Correctness Notes

- 현재 면적은 낮은 쪽 높이에 의해 제한된다.
- 높은 쪽을 움직이면 너비는 줄고 제한 높이는 낮은 쪽 그대로라 더 좋은 면적을 만들 수 없다.
- 낮은 쪽을 움직여야 더 높은 제한 높이를 만날 가능성이 있다.
- 모든 제거는 최적 후보가 될 수 없는 쪽을 버리는 것이므로 best를 놓치지 않는다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- 모든 pair를 확인하면 `O(n^2)`다.
- 더 높은 쪽이 아니라 더 낮은 쪽을 이동해야 한다.
- `Trapping Rain Water`와 비슷해 보이지만 모델이 다르다. 여기서는 두 선만 선택한다.