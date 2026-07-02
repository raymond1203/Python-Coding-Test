# Grind 75 046. Majority Element

> 배열에서 절반보다 많이 등장하는 값을 찾는 문제다. majority가 반드시 존재한다는 조건이 있으므로 Boyer-Moore Voting으로 `O(1)` 공간에 찾을 수 있다.

## Link

- [LeetCode - Majority Element](https://leetcode.com/problems/majority-element)

## Problem Model

majority element는 `n / 2`보다 많이 등장한다. 다른 값들과 한 쌍씩 상쇄해도 majority element는 끝까지 남는다.

## Pattern

- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)

## Approach

1. `candidate`와 `count`를 유지한다.
2. `count == 0`이면 현재 값을 새 candidate로 잡는다.
3. 현재 값이 candidate와 같으면 count를 늘린다.
4. 다르면 count를 줄인다.
5. majority가 존재하므로 마지막 candidate가 정답이다.

## Python 3.14 Solution

```python
class Solution:
    def majorityElement(self, nums: list[int]) -> int:
        candidate = nums[0]
        count = 0

        for num in nums:
            if count == 0:
                candidate = num

            if num == candidate:
                count += 1
            else:
                count -= 1

        return candidate
```

## Correctness Notes

- candidate와 다른 값이 만나면 둘을 한 쌍으로 제거하는 효과가 난다.
- majority element는 전체의 절반보다 많으므로 다른 모든 값과 상쇄해도 최소 하나 이상 남는다.
- `count`가 0이 되는 순간 이전 구간은 완전히 상쇄되었으므로 이후 구간에서 다시 후보를 잡아도 정답은 보존된다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- majority가 반드시 존재한다는 조건이 없으면 마지막 candidate를 한 번 더 검증해야 한다.
- Counter 풀이도 가능하지만 공간이 `O(k)`다.
- `count == 0`일 때 candidate를 바꾼 뒤 같은 반복에서 count 갱신까지 해야 한다.