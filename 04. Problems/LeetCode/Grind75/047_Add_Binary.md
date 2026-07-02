# Grind 75 047. Add Binary

> 두 binary string을 오른쪽에서 왼쪽으로 더하는 문제다. 일반 덧셈처럼 carry를 유지하면 된다.

## Link

- [LeetCode - Add Binary](https://leetcode.com/problems/add-binary)

## Problem Model

각 자리의 합은 `bit_a + bit_b + carry`다. 결과 bit는 `total % 2`, 다음 carry는 `total // 2`다.

## Pattern

- [String](../../../01.%20Data%20Structures/02.%20String.md)
- [Bit Manipulation](../../../02.%20Algorithms/11.%20Bit%20Manipulation.md)
- [Math](../../../02.%20Algorithms/12.%20Math.md)

## Approach

1. 두 문자열의 마지막 index에서 시작한다.
2. index가 남아 있거나 carry가 있으면 반복한다.
3. 현재 자리 bit를 더한다.
4. 결과 bit를 list에 append한다.
5. 마지막에 뒤집어 문자열로 만든다.

## Python 3.14 Solution

```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        i, j = len(a) - 1, len(b) - 1
        carry = 0
        result: list[str] = []

        while i >= 0 or j >= 0 or carry:
            total = carry

            if i >= 0:
                total += ord(a[i]) - ord("0")
                i -= 1
            if j >= 0:
                total += ord(b[j]) - ord("0")
                j -= 1

            result.append(str(total % 2))
            carry = total // 2

        return "".join(reversed(result))
```

## Correctness Notes

- 각 반복은 현재 가장 낮은 미처리 자리의 덧셈을 정확히 수행한다.
- `carry`는 이전 낮은 자리에서 넘어온 값을 보존한다.
- 두 문자열이 모두 끝나도 carry가 남으면 새 자리가 필요하므로 반복 조건에 포함한다.
- result는 낮은 자리부터 쌓이므로 마지막에 뒤집으면 올바른 binary string이 된다.

## Complexity

- Time: `O(max(len(a), len(b)))`
- Space: `O(max(len(a), len(b)))`

## Mistake Notes

- Python의 `bin(int(a, 2) + int(b, 2))`는 간단하지만, 자리 덧셈 구현력을 보여주기 어렵다.
- carry 처리 때문에 반복 조건에 `or carry`가 필요하다.
- 문자열 앞에 계속 붙이면 `O(n^2)`가 될 수 있으므로 list에 append 후 reverse한다.