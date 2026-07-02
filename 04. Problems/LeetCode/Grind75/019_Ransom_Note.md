# Grind 75 019. Ransom Note

> 필요한 문자 수를 magazine이 충분히 공급할 수 있는지 확인하는 counting 문제다. 순서는 중요하지 않고 빈도만 중요하다.

## Link

- [LeetCode - Ransom Note](https://leetcode.com/problems/ransom-note)

## Problem Model

`ransomNote`를 만들기 위해 각 문자가 몇 개 필요한지 세고, `magazine`이 그 수량을 모두 만족하는지 확인한다.

## Pattern

- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [Hashing and Counting](../../../03.%20Problem%20Solving%20Patterns/04.%20Hashing%20and%20Counting.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `ransomNote`의 문자 빈도를 센다.
2. `magazine`을 순회하며 필요한 문자가 있으면 개수를 줄인다.
3. 어떤 문자의 필요 개수가 0이 되면 제거한다.
4. 필요한 문자가 모두 사라지면 만들 수 있다.

## Python 3.14 Solution

```python
from collections import Counter


class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        need = Counter(ransomNote)

        for char in magazine:
            if char not in need:
                continue

            need[char] -= 1
            if need[char] == 0:
                del need[char]

            if not need:
                return True

        return not need
```

## Correctness Notes

- `need[c]`는 아직 더 필요한 문자 `c`의 개수다.
- magazine의 문자를 하나 사용할 때마다 해당 필요량을 정확히 1 줄인다.
- 모든 필요량이 0이 되면 ransom note의 모든 문자를 구성할 수 있다.
- 끝까지 남은 필요 문자가 있다면 magazine이 부족하므로 불가능하다.

## Complexity

- Time: `O(r + m)`, where `r = len(ransomNote)` and `m = len(magazine)`
- Space: `O(k)`, distinct needed characters.

## Mistake Notes

- 문자열 정렬로도 풀 수 있지만 counting이 더 직접적이다.
- 같은 문자가 여러 번 필요할 수 있으므로 set만으로는 부족하다.
- `magazine`의 문자를 재사용할 수 없다는 점이 핵심이다.