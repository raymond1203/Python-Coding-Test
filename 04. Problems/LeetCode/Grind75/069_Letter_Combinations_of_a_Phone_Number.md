# Grind 75 069. Letter Combinations of a Phone Number

> 전화번호 digit들이 만들 수 있는 모든 문자 조합을 생성하는 backtracking 문제다. 각 digit마다 가능한 문자 중 하나를 선택하며 깊이를 늘린다.

## Link

- [LeetCode - Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number)

## Problem Model

각 digit은 여러 문자에 매핑된다. 입력 digit 순서를 유지하면서 각 자리에서 문자 하나씩 선택해 모든 조합을 만든다.

## Pattern

- [Backtracking](../../../02.%20Algorithms/05.%20Backtracking.md)
- [Backtracking Search Patterns](../../../03.%20Problem%20Solving%20Patterns/09.%20Backtracking%20Search%20Patterns.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)

## Approach

1. digit to letters mapping을 준비한다.
2. 입력이 비어 있으면 빈 list를 반환한다.
3. 현재 index의 digit에 해당하는 문자를 하나씩 선택한다.
4. path 길이가 digits 길이와 같아지면 조합을 결과에 추가한다.

## Python 3.14 Solution

```python
class Solution:
    def letterCombinations(self, digits: str) -> list[str]:
        if not digits:
            return []

        mapping = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }
        result: list[str] = []
        path: list[str] = []

        def backtrack(index: int) -> None:
            if index == len(digits):
                result.append("".join(path))
                return

            for char in mapping[digits[index]]:
                path.append(char)
                backtrack(index + 1)
                path.pop()

        backtrack(0)
        return result
```

## Correctness Notes

- 각 recursion depth는 digits의 한 자리를 의미한다.
- 해당 digit의 모든 가능한 문자를 시도하므로 모든 조합을 생성한다.
- path가 digits 길이에 도달하면 각 digit에서 하나씩 선택한 완전한 조합이다.
- backtracking으로 선택을 되돌리므로 다른 조합 탐색에 상태가 섞이지 않는다.

## Complexity

- Time: `O(4^n * n)` in the worst case.
- Space: `O(n)` auxiliary, excluding output.

## Mistake Notes

- 입력이 빈 문자열이면 `[]`를 반환한다.
- digit `7`, `9`는 문자가 4개다.
- 조합 문자열을 만들 때 path를 join해야 한다.