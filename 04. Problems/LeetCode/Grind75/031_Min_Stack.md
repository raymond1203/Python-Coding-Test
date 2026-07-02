# Grind 75 031. Min Stack

> 일반 stack 연산과 함께 현재 최솟값을 `O(1)`에 반환해야 하는 설계 문제다. 값 stack과 최소값 stack을 분리하면 각 시점의 최소 상태를 보존할 수 있다.

## Link

- [LeetCode - Min Stack](https://leetcode.com/problems/min-stack)

## Problem Model

`push`, `pop`, `top`, `getMin` 모두 상수 시간이어야 한다. 전체 stack을 매번 스캔해서 최소값을 찾으면 안 된다.

## Pattern

- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)
- [Design with Multiple Structures](../../../03.%20Problem%20Solving%20Patterns/11.%20Design%20with%20Multiple%20Structures.md)

## Approach

- `stack`: 실제 값들을 저장한다.
- `min_stack`: 현재까지의 최소값 후보들을 저장한다.
- 새 값이 현재 최소값 이하이면 `min_stack`에도 push한다.
- pop한 값이 현재 최소값과 같으면 `min_stack`에서도 pop한다.

## Python 3.14 Solution

```python
class MinStack:
    def __init__(self):
        self.stack: list[int] = []
        self.min_stack: list[int] = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self) -> None:
        value = self.stack.pop()
        if value == self.min_stack[-1]:
            self.min_stack.pop()

    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.min_stack[-1]
```

## Correctness Notes

- `min_stack[-1]`은 현재 stack에 남아 있는 값 중 최솟값이다.
- 같은 최솟값이 여러 번 들어올 수 있으므로 `<=` 조건으로 중복 최소값도 저장해야 한다.
- 최솟값이 pop될 때만 `min_stack`도 함께 pop되므로 이전 최솟값 상태가 복원된다.

## Complexity

- Time: all operations `O(1)`
- Space: `O(n)`

## Mistake Notes

- `push`에서 `<`만 쓰면 같은 최소값이 여러 개 있을 때 하나를 pop한 뒤 최소 상태가 깨질 수 있다.
- `getMin`에서 `min(self.stack)`을 호출하면 `O(n)`이다.
- `pop`은 문제 조건상 비어 있지 않을 때 호출된다고 가정할 수 있다.