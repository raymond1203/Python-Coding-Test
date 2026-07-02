# Grind 75 032. Reverse Linked List

> 단방향 linked list의 화살표 방향을 모두 뒤집는 문제다. 현재 노드의 다음 노드를 잃지 않도록 `next_node`를 먼저 저장한 뒤 포인터를 뒤집는다.

## Link

- [LeetCode - Reverse Linked List](https://leetcode.com/problems/reverse-linked-list)

## Problem Model

각 노드의 `next`를 이전 노드로 바꾸면 list가 역순이 된다. 순회 중 아직 처리하지 않은 나머지 list의 시작점을 보존해야 한다.

## Pattern

- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)
- [In-place Reversal](../../../03.%20Problem%20Solving%20Patterns/13.%20In-place%20Reversal.md)

## Approach

1. `prev = None`, `current = head`로 시작한다.
2. `next_node = current.next`를 저장한다.
3. `current.next = prev`로 방향을 뒤집는다.
4. `prev`, `current`를 한 칸 전진시킨다.
5. 끝나면 `prev`가 새 head다.

## Python 3.14 Solution

```python
class ListNode:
    def __init__(self, val: int = 0, next: "ListNode | None" = None):
        self.val = val
        self.next = next


class Solution:
    def reverseList(self, head: ListNode | None) -> ListNode | None:
        prev: ListNode | None = None
        current = head

        while current is not None:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node

        return prev
```

## Correctness Notes

- 반복 시작 시 `prev`는 이미 뒤집힌 prefix의 head다.
- `current.next = prev`를 수행하면 현재 노드가 뒤집힌 prefix 앞에 붙는다.
- `next_node`를 미리 저장했으므로 아직 처리하지 않은 suffix를 잃지 않는다.
- 모든 노드를 처리하면 전체 list가 뒤집히고 `prev`가 새 head가 된다.

## Complexity

- Time: `O(n)`
- Space: `O(1)`

## Mistake Notes

- `current.next`를 먼저 바꾸면 원래 다음 노드를 잃어버릴 수 있다.
- dummy node가 필요하지 않은 대표적인 linked list 기본 문제다.
- 재귀 풀이도 가능하지만 iterative 풀이가 공간 복잡도와 안정성 측면에서 좋다.