# Grind 75 017. Implement Queue using Stacks

> Stack 두 개로 Queue의 FIFO 동작을 만드는 설계 문제다. 입력용 stack과 출력용 stack을 분리하면 순서를 뒤집는 비용을 필요한 순간에만 지불할 수 있다.

## Link

- [LeetCode - Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks)

## Problem Model

Queue는 먼저 들어온 값이 먼저 나와야 한다. Stack은 나중에 들어온 값이 먼저 나온다. Stack에서 Stack으로 값을 옮기면 순서가 한 번 뒤집히므로 FIFO 순서를 만들 수 있다.

## Pattern

- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)
- [Design with Multiple Structures](../../../03.%20Problem%20Solving%20Patterns/11.%20Design%20with%20Multiple%20Structures.md)

## Approach

- `in_stack`: 새로 들어오는 값을 쌓는다.
- `out_stack`: pop/peek에 사용할 값을 쌓는다.
- pop/peek 시 `out_stack`이 비어 있으면 `in_stack`의 모든 값을 옮긴다.
- 한 번 옮긴 값들은 queue 순서대로 `out_stack`에서 빠진다.

## Python 3.14 Solution

```python
class MyQueue:
    def __init__(self):
        self.in_stack: list[int] = []
        self.out_stack: list[int] = []

    def push(self, x: int) -> None:
        self.in_stack.append(x)

    def pop(self) -> int:
        self._move_if_needed()
        return self.out_stack.pop()

    def peek(self) -> int:
        self._move_if_needed()
        return self.out_stack[-1]

    def empty(self) -> bool:
        return not self.in_stack and not self.out_stack

    def _move_if_needed(self) -> None:
        if self.out_stack:
            return

        while self.in_stack:
            self.out_stack.append(self.in_stack.pop())
```

## Correctness Notes

- `out_stack`이 비어 있지 않다면 그 top은 현재 queue의 front다.
- `out_stack`이 비었을 때 `in_stack` 전체를 옮기면, 가장 오래전에 들어온 값이 `out_stack`의 top으로 온다.
- 각 값은 `in_stack`에 한 번 push되고, 최대 한 번 `out_stack`으로 이동하며, 한 번 pop된다.

## Complexity

- `push`: `O(1)`
- `pop`, `peek`: amortized `O(1)`, worst-case `O(n)`
- Space: `O(n)`

## Mistake Notes

- 매 `push`마다 모든 값을 옮기면 비효율적이다.
- `peek`도 `out_stack`이 비어 있으면 이동을 수행해야 한다.
- amortized complexity 설명을 반드시 붙이면 인터뷰 답변의 설득력이 좋아진다.