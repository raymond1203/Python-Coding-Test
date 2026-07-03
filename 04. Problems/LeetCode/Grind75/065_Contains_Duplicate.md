# Grind 75 065. Contains Duplicate

> 배열 안에 같은 값이 두 번 이상 등장하는지 확인하는 문제다. 이미 본 값을 set에 저장하면 중복 발견 즉시 종료할 수 있다.

## Link

- [LeetCode - Contains Duplicate](https://leetcode.com/problems/contains-duplicate)

## Problem Model

순서는 중요하지 않고 값의 존재 여부만 중요하다. 같은 값을 다시 만나면 duplicate가 존재한다.

## Pattern

- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

1. 빈 set `seen`을 만든다.
2. 배열을 순회한다.
3. 현재 값이 `seen`에 있으면 `True`를 반환한다.
4. 없으면 set에 추가한다.
5. 끝까지 중복을 못 찾으면 `False`다.

## Python 3.14 Solution

```python
class Solution:
    def containsDuplicate(self, nums: list[int]) -> bool:
        seen: set[int] = set()

        for num in nums:
            if num in seen:
                return True
            seen.add(num)

        return False
```

## Correctness Notes

- `seen`에는 현재 index 이전에 등장한 모든 값이 들어 있다.
- 현재 값이 `seen`에 있으면 이전에 같은 값이 있었다는 뜻이다.
- 현재 값이 없으면 이후 중복 검사를 위해 저장한다.
- 모든 값을 처리했는데 중복이 없으면 어떤 값도 두 번 등장하지 않았다.

## Complexity

- Time: `O(n)` average
- Space: `O(n)`

## Mistake Notes

- 정렬 후 인접 비교도 가능하지만 `O(n log n)`이고 원본 순서를 바꿀 수 있다.
- `len(nums) != len(set(nums))`도 간단하지만 조기 종료는 하지 못한다.