# Grind 75 071. Find All Anagrams in a String

> 문자열 `s`에서 `p`의 anagram이 시작되는 모든 index를 찾는 문제다. 고정 길이 sliding window의 문자 빈도를 `p`의 빈도와 비교하면 된다.

## Link

- [LeetCode - Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string)

## Problem Model

Anagram은 문자 빈도가 같은 문자열이다. 따라서 길이 `len(p)`의 window를 움직이며 빈도 차이를 추적한다.

## Pattern

- [Sliding Window](../../../03.%20Problem%20Solving%20Patterns/02.%20Sliding%20Window.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `need`에 `p`의 문자 빈도를 저장한다.
2. `window`에 현재 window의 빈도를 저장한다.
3. 오른쪽으로 한 문자 추가한다.
4. window 크기가 `len(p)`를 넘으면 왼쪽 문자를 제거한다.
5. 크기가 같고 빈도가 같으면 시작 index를 기록한다.

## Python 3.14 Solution

```python
from collections import Counter, defaultdict


class Solution:
    def findAnagrams(self, s: str, p: str) -> list[int]:
        if len(p) > len(s):
            return []

        need = Counter(p)
        window: dict[str, int] = defaultdict(int)
        result: list[int] = []
        left = 0

        for right, char in enumerate(s):
            window[char] += 1

            if right - left + 1 > len(p):
                left_char = s[left]
                window[left_char] -= 1
                if window[left_char] == 0:
                    del window[left_char]
                left += 1

            if right - left + 1 == len(p) and window == need:
                result.append(left)

        return result
```

## Correctness Notes

- window 크기를 항상 `len(p)` 이하로 유지한다.
- 크기가 정확히 `len(p)`일 때 window와 `p`의 빈도가 같으면 anagram이다.
- 오른쪽을 한 칸씩 늘리고 왼쪽을 필요한 만큼 제거하므로 모든 길이 `len(p)` substring을 한 번씩 확인한다.
- 빈도가 0이 된 문자를 삭제해야 Counter와 dict 비교가 정확해진다.

## Complexity

- Time: `O(n * k)` if dict equality compares distinct characters; lowercase alphabet이면 사실상 `O(n)`.
- Space: `O(k)`, distinct characters.

## Mistake Notes

- 정렬로 각 window를 비교하면 느리다.
- window에서 빠진 문자의 count가 0이면 key를 제거해야 한다.
- 고정 길이 window라서 Minimum Window Substring보다 shrink 조건이 단순하다.