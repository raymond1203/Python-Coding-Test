# Grind 75 001. Two Sum

> 한 번 본 값을 다시 찾아야 하므로, 값에서 index로 가는 hash table을 유지하는 문제다.

## Link

- [LeetCode - Two Sum](https://leetcode.com/problems/two-sum)

## Problem Model

배열에서 두 수를 골라 target을 만들어야 한다. 핵심은 현재 값 `x`를 볼 때 이미 지나온 값 중 `target - x`가 있었는지 빠르게 확인하는 것이다.

## Pattern

- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)

## Approach

1. `seen[value] = index`를 저장한다.
2. 왼쪽에서 오른쪽으로 순회한다.
3. 현재 값의 complement가 이미 `seen`에 있으면 두 index를 반환한다.
4. 없으면 현재 값을 `seen`에 저장한다.

같은 원소를 두 번 쓰면 안 되므로, 현재 값을 저장하기 전에 complement를 먼저 확인한다.

## Python 3.14 Solution

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        seen: dict[int, int] = {}

        for i, num in enumerate(nums):
            need = target - num
            if need in seen:
                return [seen[need], i]
            seen[num] = i

        return []
```

## Complexity

- Time: O(n)
- Space: O(n)

## Mistake Points

- 현재 값을 먼저 저장하면 같은 index를 두 번 사용할 수 있다.
- 중복 값이 있을 수 있으므로 value 하나에 index 하나만 저장해도 충분한지 확인한다. 이 문제는 정답 하나를 반환하면 되므로 충분하다.

## Related Notes

- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
