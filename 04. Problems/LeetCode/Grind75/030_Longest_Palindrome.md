# Grind 75 030. Longest Palindrome

> 주어진 문자들로 만들 수 있는 가장 긴 palindrome의 길이를 구하는 문제다. 순서는 재배열 가능하므로 핵심은 각 문자의 빈도다.

## Link

- [LeetCode - Longest Palindrome](https://leetcode.com/problems/longest-palindrome)

## Problem Model

Palindrome은 양쪽에 같은 문자를 쌍으로 배치한다. 홀수 개 남는 문자는 전체 palindrome의 중앙에 최대 하나만 둘 수 있다.

## Pattern

- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. 각 문자의 빈도를 센다.
2. 각 빈도에서 짝수 부분 `count // 2 * 2`를 길이에 더한다.
3. 홀수 빈도가 하나라도 있으면 중앙 문자 하나를 추가한다.

## Python 3.14 Solution

```python
from collections import Counter


class Solution:
    def longestPalindrome(self, s: str) -> int:
        counts = Counter(s)
        length = 0
        has_odd = False

        for count in counts.values():
            length += (count // 2) * 2
            if count % 2 == 1:
                has_odd = True

        return length + 1 if has_odd else length
```

## Correctness Notes

- 각 문자는 palindrome의 양쪽에 쌍으로 배치될 수 있으므로 짝수 개수만큼은 항상 사용할 수 있다.
- 홀수 빈도 문자마다 하나씩 남지만, 중앙 위치는 하나뿐이므로 추가 가능한 홀수 문자는 최대 하나다.
- 모든 문자 빈도에 대해 사용할 수 있는 최대 개수를 더하므로 전체 최장 길이가 된다.

## Complexity

- Time: `O(n)`
- Space: `O(k)`, distinct characters.

## Mistake Notes

- 실제 palindrome 문자열을 만들 필요는 없다. 길이만 구하면 된다.
- 홀수 빈도가 여러 개 있어도 중앙 문자는 하나만 가능하다.
- 대문자와 소문자는 다른 문자로 취급된다.