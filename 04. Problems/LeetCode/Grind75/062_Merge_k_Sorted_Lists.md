# Grind 75 062. Merge k Sorted Lists

> 여러 개의 sorted linked list를 하나로 합치는 문제다. 각 list의 현재 head 중 가장 작은 값을 계속 선택하면 되므로 min-heap이 잘 맞는다.

## Link

- [LeetCode - Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists)

## Problem Model

각 linked list는 오름차순으로 정렬되어 있다. 전체 merged list도 오름차순이어야 한다. 항상 k개의 후보 head 중 최소 노드를 선택하면 된다.

## Pattern

- [Heap](../../../01.%20Data%20Structures/10.%20Heap.md)
- [Top K with Heap](../../../03.%20Problem%20Solving%20Patterns/18.%20Top%20K%20with%20Heap.md)
- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)

## Approach

1. 각 list의 head를 heap에 넣는다.
2. heap에는 `(node.val, unique_id, node)`를 넣어 값이 같을 때 비교 문제를 피한다.
3. heap에서 가장 작은 node를 꺼내 merged list 뒤에 붙인다.
4. 꺼낸 node의 next가 있으면 heap에 넣는다.
5. heap이 빌 때까지 반복한다.

## Python 3.14 Solution

```python
import heapq
from itertools import count


class ListNode:
    def __init__(self, val: int = 0, next: "ListNode | None" = None):
        self.val = val
        self.next = next


class Solution:
    def mergeKLists(self, lists: list[ListNode | None]) -> ListNode | None:
        heap: list[tuple[int, int, ListNode]] = []
        unique = count()

        for node in lists:
            if node is not None:
                heapq.heappush(heap, (node.val, next(unique), node))

        dummy = ListNode()
        tail = dummy

        while heap:
            _, _, node = heapq.heappop(heap)
            tail.next = node
            tail = tail.next

            if node.next is not None:
                heapq.heappush(heap, (node.next.val, next(unique), node.next))

        return dummy.next
```

## Correctness Notes

- heap에는 아직 merged list에 붙지 않은 각 list의 최소 후보가 들어 있다.
- heap에서 꺼낸 node는 모든 남은 후보 중 최솟값이므로 다음 위치에 두는 것이 안전하다.
- node를 꺼낸 뒤 같은 list의 다음 node를 넣으면 그 list의 다음 최소 후보가 유지된다.
- 모든 node가 한 번씩 pop되어 merged list에 붙으므로 전체 정렬 list가 완성된다.

## Complexity

- Time: `O(N log k)`, where `N` is total nodes.
- Space: `O(k)` for the heap.

## Mistake Notes

- 값이 같은 node가 있으면 Python heap이 ListNode끼리 비교하려 할 수 있다. unique id를 함께 넣어야 안전하다.
- 빈 list와 `None` head를 건너뛰어야 한다.
- divide and conquer merge도 좋은 대안이다.