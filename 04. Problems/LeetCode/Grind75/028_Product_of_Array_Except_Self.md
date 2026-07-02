# Grind 75 028. Product of Array Except Self

> 각 index를 제외한 나머지 곱을 구하는 문제다. 나눗셈 없이 왼쪽 prefix 곱과 오른쪽 suffix 곱을 곱하면 된다.

## Link

- [LeetCode - Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self)

## Problem Model

정답 `answer[i]`는 `nums[i]` 왼쪽 모든 값의 곱과 오른쪽 모든 값의 곱을 곱한 값이다. 자기 자신은 어느 쪽에도 포함하지 않는다.

## Pattern

- [Prefix Sum and Difference Array](../../../03.%20Problem%20Solving%20Patterns/03.%20Prefix%20Sum%20and%20Difference%20Array.md)
- [Array and List](../../../01.%20Data%20Structures/01.%20Array%20and%20List.md)

## Approach

1. `answer[i]`에 먼저 `i` 왼쪽의 prefix product를 저장한다.
2. 오른쪽에서 왼쪽으로 순회하며 suffix product를 곱한다.
3. 순회 후 `answer[i]`는 왼쪽 곱과 오른쪽 곱의 곱이 된다.

## Python 3.14 Solution

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        n = len(nums)
        answer = [1] * n

        prefix = 1
        for i in range(n):
            answer[i] = prefix
            prefix *= nums[i]

        suffix = 1
        for i in range(n - 1, -1, -1):
            answer[i] *= suffix
            suffix *= nums[i]

        return answer
```

## Correctness Notes

- 첫 번째 순회 후 `answer[i]`는 `nums[0] * ... * nums[i - 1]`이다.
- 두 번째 순회에서 `suffix`는 `nums[i + 1] * ... * nums[n - 1]`이다.
- 둘을 곱하면 정확히 `nums[i]`를 제외한 모든 원소의 곱이 된다.
- 0이 포함되어도 나눗셈을 쓰지 않으므로 자연스럽게 처리된다.

## Complexity

- Time: `O(n)`
- Space: `O(1)` auxiliary space, excluding output.

## Mistake Notes

- 전체 곱을 구한 뒤 나누는 방식은 0 때문에 위험하고, 문제 조건에서도 나눗셈을 피하라는 의도가 있다.
- prefix를 저장할 때 현재 `nums[i]`를 곱하기 전에 `answer[i]`에 넣어야 자기 자신이 제외된다.
- suffix도 마찬가지로 현재 값을 곱하기 전에 `answer[i]`에 반영한다.