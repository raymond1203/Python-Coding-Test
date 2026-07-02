# Grind 75 044. Trapping Rain Water

> 각 위치에 고이는 물은 왼쪽 최고 벽과 오른쪽 최고 벽 중 낮은 쪽에 의해 결정된다. Two pointers로 더 낮은 벽 쪽을 확정해 나가면 `O(1)` 공간으로 풀 수 있다.

## Link

- [LeetCode - Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water)

## Problem Model

index `i`에 고일 수 있는 물의 양은 `min(max_left[i], max_right[i]) - height[i]`다. 단, 양수일 때만 물이 고인다.

## Pattern

- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)
- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)

## Approach

1. 양끝에서 `left`, `right`를 시작한다.
2. `left_max`, `right_max`를 유지한다.
3. 더 낮은 쪽의 pointer를 처리한다.
4. 왼쪽이 낮으면 오른쪽에는 최소한 그보다 높은 경계가 있으므로 왼쪽 물 양을 확정할 수 있다.
5. 오른쪽도 대칭적으로 처리한다.

## Python 3.14 Solution

```python
class Solution:
    def trap(self, height: list[int]) -> int:
        left, right = 0, len(height) - 1
        left_max = right_max = 0
        water = 0

        while left < right:
            if height[left] <= height[right]:
                left_max = max(left_max, height[left])
                water += left_max - height[left]
                left += 1
            else:
                right_max = max(right_max, height[right])
                water += right_max - height[right]
                right -= 1

        return water
```

## Correctness Notes

- `height[left] <= height[right]`이면 왼쪽 위치의 오른쪽 경계는 적어도 `height[left]` 이상이다.
- 따라서 왼쪽 위치에 고이는 물은 현재까지의 `left_max`로 확정할 수 있다.
- 오른쪽이 더 낮은 경우도 같은 논리로 `right_max`를 기준으로 확정한다.
- 매 반복마다 한 위치의 물 양을 정확히 확정하므로 전체 합이 정답이다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- `left_max - height[left]`는 `left_max`를 먼저 갱신하면 음수가 되지 않는다.
- prefix/suffix 배열 풀이도 가능하지만 공간이 `O(n)`이다.
- monotonic stack 풀이도 대표 풀이지만, two pointers가 더 간결하다.