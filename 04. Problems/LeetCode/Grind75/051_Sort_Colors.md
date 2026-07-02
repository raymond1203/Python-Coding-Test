# Grind 75 051. Sort Colors

> 0, 1, 2 세 값만 있는 배열을 in-place 정렬하는 문제다. Dutch National Flag 알고리즘으로 0 영역, 미분류 영역, 2 영역을 한 번의 scan으로 나눌 수 있다.

## Link

- [LeetCode - Sort Colors](https://leetcode.com/problems/sort-colors)

## Problem Model

배열에는 0, 1, 2만 존재한다. 추가 배열 없이 `0...0, 1...1, 2...2` 순서로 정렬해야 한다.

## Pattern

- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

세 구간을 유지한다.

- `[0, low)`는 0
- `[low, mid)`는 1
- `[mid, high]`는 아직 모름
- `(high, end]`는 2

`mid`의 값에 따라 swap한다.

## Python 3.14 Solution

```python
class Solution:
    def sortColors(self, nums: list[int]) -> None:
        low, mid, high = 0, 0, len(nums) - 1

        while mid <= high:
            if nums[mid] == 0:
                nums[low], nums[mid] = nums[mid], nums[low]
                low += 1
                mid += 1
            elif nums[mid] == 1:
                mid += 1
            else:
                nums[mid], nums[high] = nums[high], nums[mid]
                high -= 1
```

## Correctness Notes

- `low` 앞은 모두 0, `high` 뒤는 모두 2라는 invariant를 유지한다.
- `nums[mid] == 0`이면 0 영역 끝으로 보내고 `low`, `mid`를 함께 전진한다.
- `nums[mid] == 1`이면 이미 중간 영역에 맞으므로 `mid`만 전진한다.
- `nums[mid] == 2`이면 뒤로 보내지만, 새로 들어온 `nums[mid]`는 미분류이므로 `mid`를 전진하지 않는다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- 2와 swap한 뒤 `mid`를 증가시키면 새로 온 미분류 값을 건너뛸 수 있다.
- counting sort도 가능하지만, 이 문제는 in-place one-pass partition 패턴을 익히는 것이 좋다.
- 함수는 배열을 직접 수정하고 반환하지 않는다.