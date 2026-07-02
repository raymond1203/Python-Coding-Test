# Grind 75 016. Linked List Cycle

> Linked list에 cycle이 있는지 확인하는 문제다. 방문 집합으로도 풀 수 있지만, 포인터 두 개의 속도 차이를 이용하면 `O(1)` 공간으로 해결된다.

## Link

- [LeetCode - Linked List Cycle](https://leetcode.com/problems/linked-list-cycle)

## Problem Model

단방향 linked list에서 어떤 노드의 `next`가 이전 노드 중 하나를 가리키면 cycle이 존재한다. cycle이 있으면 빠른 포인터는 언젠가 느린 포인터를 따라잡는다.

## Pattern

- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)
- [Fast and Slow Pointers](../../../03.%20Problem%20Solving%20Patterns/05.%20Fast%20and%20Slow%20Pointers.md)

## Approach

1. `slow`는 한 칸, `fast`는 두 칸씩 이동한다.
2. `fast`가 끝에 도달하면 cycle이 없다.
3. 이동 중 `slow is fast`가 되면 cycle이 있다.

## Python 3.14 Solution

```python
class ListNode:
    def __init__(self, val: int = 0, next: "ListNode | None" = None):
        self.val = val
        self.next = next


class Solution:
    def hasCycle(self, head: ListNode | None) -> bool:
        slow = fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

            if slow is fast:
                return True

        return False
```

## Correctness Notes

- Cycle이 없다면 `fast`는 list 끝의 `None`에 도달한다.
- Cycle이 있다면 두 포인터가 cycle 내부에 들어간 뒤, `fast`는 매 반복마다 `slow`와의 거리를 1씩 줄이는 효과를 낸다.
- 유한한 cycle 길이 안에서 상대 거리는 반드시 0이 되므로 두 포인터는 만난다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- 노드 값 비교가 아니라 객체 동일성 비교가 필요하다. Python에서는 `is`를 사용한다.
- `fast.next.next`에 접근하기 전에 `fast`와 `fast.next`가 `None`이 아닌지 확인해야 한다.
- 방문 set 풀이도 가능하지만, 이 문제의 핵심 패턴은 fast/slow pointer다.