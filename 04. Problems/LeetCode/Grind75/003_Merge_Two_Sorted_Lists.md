# Grind 75 003. Merge Two Sorted Lists

> 두 정렬된 linked list를 앞에서부터 비교해 더 작은 node를 결과 list 뒤에 붙이는 문제다.

## Link

- [LeetCode - Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists)

## Problem Model

두 list는 이미 정렬되어 있다. 따라서 매 순간 두 head 중 더 작은 값을 고르면, 그 선택은 결과 list의 다음 원소로 안전하다.

## Pattern

- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)
- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)

## Approach

1. dummy node를 두어 head edge case를 없앤다.
2. `list1`, `list2`를 비교하면서 작은 node를 `tail.next`에 붙인다.
3. 붙인 list의 pointer를 한 칸 이동한다.
4. 한쪽이 끝나면 남은 list는 이미 정렬되어 있으므로 그대로 붙인다.

## Python 3.14 Solution

```python
from __future__ import annotations
from dataclasses import dataclass

@dataclass
class ListNode:
    val: int = 0
    next: ListNode | None = None


class Solution:
    def mergeTwoLists(
        self,
        list1: ListNode | None,
        list2: ListNode | None,
    ) -> ListNode | None:
        dummy = ListNode()
        tail = dummy

        while list1 is not None and list2 is not None:
            if list1.val <= list2.val:
                tail.next = list1
                list1 = list1.next
            else:
                tail.next = list2
                list2 = list2.next
            tail = tail.next

        tail.next = list1 if list1 is not None else list2
        return dummy.next
```

## Complexity

- Time: O(n + m)
- Space: O(1)

## Mistake Points

- dummy node를 쓰지 않으면 첫 node 처리 분기가 복잡해진다.
- 남은 list를 새로 순회할 필요 없이 그대로 붙이면 된다.
- `tail = tail.next` 이동을 빼먹으면 list가 제대로 이어지지 않는다.

## Related Notes

- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)
- [Two Pointers](../../../03.%20Problem%20Solving%20Patterns/01.%20Two%20Pointers.md)
