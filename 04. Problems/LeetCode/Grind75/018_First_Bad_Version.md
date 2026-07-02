# Grind 75 018. First Bad Version

> `False ... False, True ... True` 형태의 단조 predicate에서 첫 `True`를 찾는 문제다. 값 탐색이 아니라 boundary 탐색으로 보아야 한다.

## Link

- [LeetCode - First Bad Version](https://leetcode.com/problems/first-bad-version)

## Problem Model

`isBadVersion(version)`은 어느 시점부터 계속 `True`가 된다. 즉, 답은 bad version이 시작되는 가장 왼쪽 경계다.

## Pattern

- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Binary Search on Answer](../../../03.%20Problem%20Solving%20Patterns/21.%20Binary%20Search%20on%20Answer.md)

## Approach

1. 탐색 범위를 `[1, n]`으로 둔다.
2. `mid`가 bad라면 답은 `mid`이거나 그 왼쪽에 있다.
3. `mid`가 good이라면 답은 `mid + 1` 이후에 있다.
4. `left == right`가 되면 첫 bad version이다.

## Python 3.14 Solution

```python
def isBadVersion(version: int) -> bool:
    ...


class Solution:
    def firstBadVersion(self, n: int) -> int:
        left, right = 1, n

        while left < right:
            mid = left + (right - left) // 2

            if isBadVersion(mid):
                right = mid
            else:
                left = mid + 1

        return left
```

## Correctness Notes

- 반복 중 답은 항상 `[left, right]` 안에 있다.
- `mid`가 bad면 첫 bad는 `mid`보다 오른쪽일 수 없으므로 `right = mid`가 안전하다.
- `mid`가 good이면 `mid`와 그 왼쪽은 답이 될 수 없으므로 `left = mid + 1`이 안전하다.
- 구간이 하나로 수렴하면 그 위치가 첫 bad version이다.

## Complexity

- Time: `O(log n)` API calls
- Space: `O(1)`

## Mistake Notes

- `right = mid - 1`을 쓰면 `mid`가 첫 bad인 경우 답을 버리게 된다.
- boundary search에서는 `while left < right`와 `right = mid` 조합이 안정적이다.
- 문제의 단조성을 먼저 말하면 풀이가 자연스럽다.