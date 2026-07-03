# Grind 75 058. Longest Palindromic Substring

> 가장 긴 palindrome substring을 찾는 문제다. Palindrome은 중심을 기준으로 좌우가 대칭이므로, 각 index를 중심으로 확장하면 모든 후보를 확인할 수 있다.

## Link

- [LeetCode - Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring)

## Problem Model

Substring은 연속된 구간이다. Palindrome의 중심은 한 문자일 수도 있고, 두 문자 사이일 수도 있다.

## Pattern

- [String](../../../01.%20Data%20Structures/02.%20String.md)
- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
- [Sequence DP](../../../03.%20Problem%20Solving%20Patterns/24.%20Sequence%20DP.md)

## Approach

1. 각 index `i`를 홀수 길이 palindrome의 중심으로 확장한다.
2. `i`, `i + 1`을 짝수 길이 palindrome의 중심으로 확장한다.
3. 확장 결과가 현재 best보다 길면 갱신한다.
4. 모든 중심을 확인한 뒤 best substring을 반환한다.

## Python 3.14 Solution

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        best_left = 0
        best_right = 0

        def expand(left: int, right: int) -> tuple[int, int]:
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            return left + 1, right - 1

        for center in range(len(s)):
            left1, right1 = expand(center, center)
            left2, right2 = expand(center, center + 1)

            if right1 - left1 > best_right - best_left:
                best_left, best_right = left1, right1
            if right2 - left2 > best_right - best_left:
                best_left, best_right = left2, right2

        return s[best_left : best_right + 1]
```

## Correctness Notes

- 모든 palindrome substring은 홀수 중심 또는 짝수 중심을 하나 가진다.
- `expand`는 해당 중심에서 가능한 최대 palindrome 범위를 찾는다.
- 모든 중심의 최대 palindrome을 비교하므로 전체 최장 palindrome을 놓치지 않는다.
- 반환 구간은 확장이 실패하기 직전의 유효 범위다.

## Complexity

- Time: `O(n^2)`
- Space: `O(1)`

## Mistake Notes

- 짝수 길이 palindrome 중심도 반드시 확인해야 한다.
- 이 문제는 가장 긴 palindromic subsequence가 아니라 substring이다.
- Manacher 알고리즘은 `O(n)`이지만 인터뷰 기본 풀이로는 center expansion이 적절하다.