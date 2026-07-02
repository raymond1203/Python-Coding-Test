# Grind 75 005. Valid Palindrome

> 양끝에서 안쪽으로 이동하면서 비교하고, 비교 대상이 아닌 문자는 건너뛰는 Two Pointers 문제다.

## Link

- [LeetCode - Valid Palindrome](https://leetcode.com/problems/valid-palindrome)

## Problem Model

대소문자와 non-alphanumeric 문자를 정규화한 뒤, 문자열이 앞뒤로 같은지 확인한다. 전체 문자열을 새로 만들 수도 있지만, pointer로 직접 검사하면 추가 공간을 줄일 수 있다.

## Pattern

- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `left`, `right`를 양끝에 둔다.
2. 비교 대상이 아닌 문자는 pointer를 이동해 건너뛴다.
3. 두 문자를 lowercase로 비교한다.
4. 다르면 false, 끝까지 충돌이 없으면 true다.

## Python 3.14 Solution

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left, right = 0, len(s) - 1

        while left < right:
            while left < right and not s[left].isalnum():
                left += 1
            while left < right and not s[right].isalnum():
                right -= 1

            if s[left].lower() != s[right].lower():
                return False

            left += 1
            right -= 1

        return True
```

## Complexity

- Time: O(n)
- Space: O(1)

## Mistake Points

- `isalnum()`으로 비교 대상 문자를 정확히 제한한다.
- pointer를 건너뛴 뒤에도 `left < right` 조건을 유지해야 한다.
- 빈 문자열 또는 비교 대상 문자가 없는 문자열은 palindrome으로 처리된다.

## Related Notes

- [String](../../../01.%20Data%20Structures/02.%20String.md)
- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
