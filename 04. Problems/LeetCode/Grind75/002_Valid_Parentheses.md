# Grind 75 002. Valid Parentheses

> 가장 최근에 열린 괄호가 가장 먼저 닫혀야 하므로 Stack의 LIFO 성질을 그대로 적용한다.

## Link

- [LeetCode - Valid Parentheses](https://leetcode.com/problems/valid-parentheses)

## Problem Model

문자열을 왼쪽에서 오른쪽으로 읽으면서 여는 괄호는 stack에 쌓고, 닫는 괄호는 stack top과 짝이 맞는지 확인한다.

## Pattern

- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)

## Approach

1. 닫는 괄호에서 기대되는 여는 괄호를 mapping한다.
2. 여는 괄호는 stack에 push한다.
3. 닫는 괄호를 만나면 stack이 비어 있지 않은지 확인하고 top과 비교한다.
4. 모든 문자를 처리한 뒤 stack이 비어 있어야 valid하다.

## Python 3.14 Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        pairs = {
            ")": "(",
            "]": "[",
            "}": "{",
        }
        stack: list[str] = []

        for ch in s:
            if ch in pairs:
                if not stack or stack.pop() != pairs[ch]:
                    return False
            else:
                stack.append(ch)

        return not stack
```

## Complexity

- Time: O(n)
- Space: O(n)

## Mistake Points

- 중간에 stack이 비면 닫을 대상이 없으므로 즉시 실패다.
- 순서가 맞아야 하므로 단순 개수 counting만으로는 풀 수 없다.

## Related Notes

- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)
