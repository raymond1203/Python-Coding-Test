# Grind 75 060. Basic Calculator

> 괄호와 `+`, `-`가 있는 문자열 수식을 계산하는 문제다. 현재까지의 결과와 부호를 유지하고, 괄호를 만나면 이전 context를 stack에 저장한다.

## Link

- [LeetCode - Basic Calculator](https://leetcode.com/problems/basic-calculator)

## Problem Model

수식에는 정수, `+`, `-`, 괄호, 공백이 포함된다. 곱셈/나눗셈은 없으므로 precedence는 괄호만 신경 쓰면 된다.

## Pattern

- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)
- [String](../../../01.%20Data%20Structures/02.%20String.md)

## Approach

1. `result`는 현재 괄호 레벨의 누적값이다.
2. `sign`은 다음 숫자에 적용할 부호다.
3. 숫자를 만나면 전체 숫자를 읽고 `result += sign * number`를 수행한다.
4. `(`를 만나면 현재 `result`와 `sign`을 stack에 저장하고 새 context를 시작한다.
5. `)`를 만나면 현재 괄호 결과를 이전 sign과 result에 합친다.

## Python 3.14 Solution

```python
class Solution:
    def calculate(self, s: str) -> int:
        result = 0
        sign = 1
        stack: list[int] = []
        i = 0

        while i < len(s):
            char = s[i]

            if char.isdigit():
                number = 0
                while i < len(s) and s[i].isdigit():
                    number = number * 10 + ord(s[i]) - ord("0")
                    i += 1
                result += sign * number
                continue

            if char == "+":
                sign = 1
            elif char == "-":
                sign = -1
            elif char == "(":
                stack.append(result)
                stack.append(sign)
                result = 0
                sign = 1
            elif char == ")":
                previous_sign = stack.pop()
                previous_result = stack.pop()
                result = previous_result + previous_sign * result

            i += 1

        return result
```

## Correctness Notes

- 숫자를 읽는 순간 현재 부호가 적용되어 현재 context의 result에 반영된다.
- `(`는 새로운 독립 계산 context를 시작하므로 이전 result와 sign을 stack에 저장한다.
- `)`는 괄호 내부 결과를 하나의 숫자처럼 이전 context에 반영한다.
- 모든 문자를 처리하면 최상위 context의 result가 전체 수식 값이다.

## Complexity

- Time: `O(n)`
- Space: `O(d)`, where `d` is parenthesis depth.

## Mistake Notes

- 여러 자리 숫자를 한 번에 읽어야 한다.
- 공백은 무시한다.
- 괄호 앞의 부호를 저장하지 않으면 `-(...)` 형태를 잘못 계산한다.