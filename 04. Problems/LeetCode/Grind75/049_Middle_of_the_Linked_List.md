# Grind 75 049. Middle of the Linked List

> Linked list의 가운데 노드를 찾는 문제다. slow pointer는 한 칸, fast pointer는 두 칸씩 이동하면 fast가 끝에 도달할 때 slow가 가운데에 있다.

## Link

- [LeetCode - Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list)

## Problem Model

노드 개수가 홀수면 정확한 중앙, 짝수면 두 중앙 중 두 번째 노드를 반환해야 한다. fast/slow pointer는 이 조건을 자연스럽게 만족한다.

## Pattern

- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)
- [Fast and Slow Pointers](../../../03.%20Problem%20Solving%20Patterns/05.%20Fast%20and%20Slow%20Pointers.md)

## Approach

1. `slow`와 `fast`를 head에서 시작한다.
2. `slow`는 한 칸, `fast`는 두 칸 이동한다.
3. `fast`가 끝에 도달하면 `slow`를 반환한다.

## Python 3.14 Solution

```python
class ListNode:
    def __init__(self, val: int = 0, next: "ListNode | None" = None):
        self.val = val
        self.next = next


class Solution:
    def middleNode(self, head: ListNode | None) -> ListNode | None:
        slow = fast = head

        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next

        return slow
```

## Correctness Notes

- 반복마다 fast는 slow보다 두 배 빠르게 이동한다.
- fast가 list 끝에 도달했을 때 slow는 전체 길이의 절반만큼 이동했다.
- 짝수 길이에서는 fast가 `None`이 되는 순간 slow가 두 번째 middle을 가리킨다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- 짝수 길이에서 첫 번째 middle을 반환하는 변형과 조건이 다르다.
- `fast.next.next` 접근 전에 `fast`와 `fast.next`를 확인해야 한다.
- list 길이를 먼저 세는 two-pass 풀이보다 fast/slow가 더 간결하다.