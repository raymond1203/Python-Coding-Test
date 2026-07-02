# Grind 75 054. String to Integer (atoi)

> 문자열 앞부분을 정수로 파싱하되 공백, 부호, overflow 규칙을 정확히 처리하는 구현 문제다. 상태 전이를 단순하게 나누면 실수를 줄일 수 있다.

## Link

- [LeetCode - String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi)

## Problem Model

앞쪽 공백을 건너뛰고, 선택적 부호를 읽은 뒤, 연속된 숫자만 정수로 변환한다. 범위는 32-bit signed integer로 clamp한다.

## Pattern

- [String](../../../01.%20Data%20Structures/02.%20String.md)
- [Math](../../../02.%20Algorithms/12.%20Math.md)

## Approach

1. leading whitespace를 건너뛴다.
2. `+` 또는 `-` 부호를 처리한다.
3. 숫자가 이어지는 동안 값을 누적한다.
4. 누적 중 32-bit 범위를 넘으면 즉시 clamp한다.
5. 최종적으로 sign을 적용한다.

## Python 3.14 Solution

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        i = 0
        n = len(s)
        int_min = -(2**31)
        int_max = 2**31 - 1

        while i < n and s[i] == " ":
            i += 1

        sign = 1
        if i < n and s[i] in {"+", "-"}:
            sign = -1 if s[i] == "-" else 1
            i += 1

        value = 0
        while i < n and s[i].isdigit():
            digit = ord(s[i]) - ord("0")
            value = value * 10 + digit

            if sign == 1 and value > int_max:
                return int_max
            if sign == -1 and -value < int_min:
                return int_min

            i += 1

        return sign * value
```

## Correctness Notes

- parsing 순서는 문제 규칙과 동일하게 공백, 부호, 숫자 순서로 진행된다.
- 숫자가 아닌 문자를 만나면 그 뒤는 결과에 영향을 주지 않으므로 중단한다.
- 매 digit 반영 후 범위를 확인하므로 overflow되는 입력도 올바르게 clamp된다.
- 부호를 마지막에 적용하되, overflow check는 sign을 고려한다.

## Complexity

- Time: `O(n)` in the length of the parsed prefix.
- Space: `O(1)`

## Mistake Notes

- 부호 뒤에 숫자가 없으면 0이다.
- 문자열 중간의 공백이나 문자는 parsing을 종료시킨다.
- Python int는 overflow가 없지만 문제는 32-bit signed 범위로 clamp해야 한다.