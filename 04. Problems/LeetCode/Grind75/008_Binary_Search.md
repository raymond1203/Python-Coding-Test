# Grind 75 008. Binary Search

> 정렬된 배열에서는 “정답이 있을 수 있는 구간”만 관리하면 된다. Binary Search는 값을 찾는 코드가 아니라, 매 반복마다 불가능한 절반을 안전하게 버리는 증명이다.

## Link

- [LeetCode - Binary Search](https://leetcode.com/problems/binary-search)

## Problem Model

오름차순 배열 `nums`에서 `target`의 위치를 찾는다. 핵심은 `target`이 존재한다면 반드시 `[left, right]` 안에 있다는 invariant를 유지하는 것이다.

## Pattern

- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Binary Search on Answer](../../../03.%20Problem%20Solving%20Patterns/21.%20Binary%20Search%20on%20Answer.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

1. 닫힌 구간 `[left, right]`로 탐색 범위를 둔다.
2. `mid`를 확인한다.
3. `nums[mid]`가 작으면 `mid`와 그 왼쪽은 버린다.
4. `nums[mid]`가 크면 `mid`와 그 오른쪽은 버린다.
5. 같으면 즉시 index를 반환한다.

## Python 3.14 Solution

```python
class Solution:
    def search(self, nums: list[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid
            if nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1

        return -1
```

## Correctness Notes

- 반복 시작 시점마다 `target`이 존재한다면 반드시 `[left, right]` 안에 있다.
- `nums[mid] < target`이면 정렬 성질상 `mid` 이하에는 `target`이 없다.
- `nums[mid] > target`이면 정렬 성질상 `mid` 이상에는 `target`이 없다.
- 구간이 비면 존재하지 않는다는 뜻이므로 `-1`을 반환한다.

## Complexity

- Time: `O(log n)`
- Space: `O(1)`

## Mistake Notes

- `while left < right`와 `while left <= right`를 섞지 말 것.
- `right = len(nums)`를 쓰는 반열린 구간 버전과 `right = len(nums) - 1`을 쓰는 닫힌 구간 버전을 혼합하지 말 것.
- `mid`를 버릴 때는 반드시 `mid + 1`, `mid - 1`로 이동해야 무한 루프가 나지 않는다.

## Related Notes

- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Binary Search on Answer](../../../03.%20Problem%20Solving%20Patterns/21.%20Binary%20Search%20on%20Answer.md)
