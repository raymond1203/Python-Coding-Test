# Grind 75 036. Search in Rotated Sorted Array

> 정렬된 배열이 한 번 회전된 상태에서 target을 찾는 문제다. 전체는 깨져 있어도 매번 둘 중 한쪽 절반은 반드시 정렬되어 있다는 사실을 이용한다.

## Link

- [LeetCode - Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array)

## Problem Model

중복 없는 오름차순 배열이 pivot을 기준으로 회전되어 있다. `O(log n)`에 target index를 찾아야 하므로 binary search 구조를 유지해야 한다.

## Pattern

- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Binary Search on Answer](../../../03.%20Problem%20Solving%20Patterns/21.%20Binary%20Search%20on%20Answer.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

1. 일반 binary search처럼 `mid`를 잡는다.
2. `nums[left] <= nums[mid]`이면 왼쪽 절반은 정렬되어 있다.
3. target이 정렬된 절반의 범위 안에 있으면 그쪽으로 좁힌다.
4. 아니면 반대쪽으로 이동한다.
5. 오른쪽 절반이 정렬된 경우도 대칭적으로 처리한다.

## Python 3.14 Solution

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                return mid

            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

## Correctness Notes

- 회전 배열에서도 `left..mid` 또는 `mid..right` 중 적어도 하나는 정렬되어 있다.
- 정렬된 절반의 값 범위 안에 target이 없다면 그 절반에는 target이 존재할 수 없다.
- 매 반복마다 target이 있을 수 없는 절반을 제거하므로 답을 보존하면서 구간이 줄어든다.

## Complexity

- Time: `O(log n)`
- Space: `O(1)`

## Mistake Notes

- `nums[left] <= nums[mid]`에서 `<=`를 쓰면 left와 mid가 같은 경우도 안전하게 처리된다.
- target 범위 비교에서 한쪽은 `<`, 한쪽은 `<=`로 경계를 정확히 맞춰야 한다.
- 중복 값이 있는 변형 문제는 별도 처리가 필요하지만 이 문제는 중복이 없다.