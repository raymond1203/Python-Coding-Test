# Grind 75 024. Evaluate Reverse Polish Notation

> 후위 표기식은 연산자를 만나는 순간 직전 두 피연산자를 계산하면 된다. Stack이 표현식의 아직 계산되지 않은 값들을 보관한다.

## Link

- [LeetCode - Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation)

## Problem Model

Reverse Polish Notation에서는 숫자가 먼저 나오고 연산자가 뒤에 나온다. 연산자는 바로 앞의 두 계산 결과를 소비하고 새 결과를 만든다.

## Pattern

- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)

## Approach

1. token이 숫자면 stack에 넣는다.
2. token이 연산자면 stack에서 오른쪽 피연산자, 왼쪽 피연산자 순서로 꺼낸다.
3. 계산 결과를 다시 stack에 넣는다.
4. 모든 token 처리 후 stack의 유일한 값이 정답이다.

## Python 3.14 Solution

```python
class Solution:
    def evalRPN(self, tokens: list[str]) -> int:
        stack: list[int] = []
        operators = {"+", "-", "*", "/"}

        for token in tokens:
            if token not in operators:
                stack.append(int(token))
                continue

            right = stack.pop()
            left = stack.pop()

            if token == "+":
                stack.append(left + right)
            elif token == "-":
                stack.append(left - right)
            elif token == "*":
                stack.append(left * right)
            else:
                stack.append(int(left / right))

        return stack[-1]
```

## Correctness Notes

- stack에는 지금까지 읽은 prefix로 만들 수 있는 계산 결과들이 순서대로 저장된다.
- 연산자를 만나면 그 연산자의 두 피연산자는 stack top 두 개다.
- 계산 결과를 다시 push하면 이후 연산자의 피연산자로 사용할 수 있다.
- 모든 token을 처리하면 전체 표현식의 결과만 남는다.

## Complexity

- Time: `O(n)`
- Space: `O(n)`

## Mistake Notes

- 뺄셈과 나눗셈은 순서가 중요하다. 먼저 pop한 값이 오른쪽 피연산자다.
- Python의 `//`는 음수에서 floor division이므로 이 문제의 “0 방향 절단”과 다를 수 있다. `int(left / right)`를 사용한다.
- token이 `"-11"` 같은 음수 문자열일 수 있으므로 `token.isdigit()`만 쓰면 위험하다.