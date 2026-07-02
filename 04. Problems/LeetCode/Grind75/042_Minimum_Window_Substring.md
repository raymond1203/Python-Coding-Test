# Grind 75 042. Minimum Window Substring

> `s` 안에서 `t`의 모든 문자를 필요한 개수만큼 포함하는 가장 짧은 window를 찾는 문제다. 오른쪽으로 확장해 조건을 만족시킨 뒤, 왼쪽을 최대한 줄이는 sliding window의 대표 Hard 문제다.

## Link

- [LeetCode - Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring)

## Problem Model

`t`의 문자 빈도를 모두 만족하는 `s`의 최소 길이 substring을 찾아야 한다. 중복 문자도 필요한 개수만큼 포함해야 한다.

## Pattern

- [Sliding Window](../../../03.%20Problem%20Solving%20Patterns/02.%20Sliding%20Window.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `need`에 `t`의 문자 빈도를 저장한다.
2. `formed`는 필요 개수를 충족한 문자 종류 수다.
3. 오른쪽 포인터로 window를 확장한다.
4. 모든 종류가 충족되면 왼쪽 포인터를 줄이며 최소 길이를 갱신한다.
5. 왼쪽에서 빠진 문자가 필요 개수 미만이 되면 다시 확장한다.

## Python 3.14 Solution

```python
from collections import Counter, defaultdict


class Solution:
    def minWindow(self, s: str, t: str) -> str:
        if not t or len(t) > len(s):
            return ""

        need = Counter(t)
        window: dict[str, int] = defaultdict(int)
        required = len(need)
        formed = 0
        left = 0
        best_length = float("inf")
        best_left = 0

        for right, char in enumerate(s):
            window[char] += 1
            if char in need and window[char] == need[char]:
                formed += 1

            while formed == required:
                current_length = right - left + 1
                if current_length < best_length:
                    best_length = current_length
                    best_left = left

                left_char = s[left]
                window[left_char] -= 1
                if left_char in need and window[left_char] < need[left_char]:
                    formed -= 1
                left += 1

        if best_length == float("inf"):
            return ""
        return s[best_left : best_left + int(best_length)]
```

## Correctness Notes

- `formed == required`이면 현재 window는 `t`의 모든 문자 필요량을 만족한다.
- 조건을 만족하는 동안 왼쪽을 줄이므로 같은 `right`에 대해 가능한 가장 짧은 유효 window를 찾는다.
- 문자가 필요 개수 미만으로 빠지는 순간 window는 더 이상 유효하지 않으므로 확장을 재개한다.
- 모든 오른쪽 끝점에 대해 최소 유효 window를 고려하므로 전체 최솟값을 찾는다.

## Complexity

- Time: `O(len(s) + len(t))`
- Space: `O(k)`, distinct characters in `s` and `t`.

## Mistake Notes

- `t`에 중복 문자가 있을 수 있으므로 set만으로는 부족하다.
- window 길이를 갱신한 뒤 왼쪽을 줄여야 현재 유효 window를 놓치지 않는다.
- `formed`는 총 문자 수가 아니라 필요 개수를 만족한 문자 종류 수다.