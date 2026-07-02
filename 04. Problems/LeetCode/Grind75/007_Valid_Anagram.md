# Grind 75 007. Valid Anagram

> 두 문자열이 같은 문자 빈도를 가지는지 확인하는 counting 문제다.

## Link

- [LeetCode - Valid Anagram](https://leetcode.com/problems/valid-anagram)

## Problem Model

anagram 여부는 문자 순서가 아니라 각 문자의 등장 횟수로 결정된다. 따라서 두 문자열의 길이가 다르면 즉시 false이고, 길이가 같으면 빈도 map을 비교하면 된다.

## Pattern

- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. 길이가 다르면 false다.
2. 각 문자열의 문자 빈도를 센다.
3. 두 빈도 map이 같으면 anagram이다.

## Python 3.14 Solution

```python
from collections import Counter


class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

## Complexity

- Time: O(n)
- Space: O(k), distinct character count

## Mistake Points

- 정렬로도 풀 수 있지만 O(n log n)이다.
- alphabet이 고정된 lowercase라면 길이 26 배열로도 풀 수 있다.
- Unicode 범위를 일반화하면 hash map 방식이 더 자연스럽다.

## Related Notes

- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
