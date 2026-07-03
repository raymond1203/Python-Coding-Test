# Grind 75 074. LRU Cache

> 정해진 capacity 안에서 가장 오래 사용되지 않은 key를 제거하는 cache 설계 문제다. `O(1)` 조회를 위한 hash map과 `O(1)` 순서 갱신을 위한 doubly linked list를 함께 사용한다.

## Link

- [LeetCode - LRU Cache](https://leetcode.com/problems/lru-cache)

## Problem Model

`get` 또는 `put`으로 접근된 key는 가장 최근에 사용된 key가 된다. capacity를 초과하면 가장 오래 사용되지 않은 key를 제거해야 한다.

## Pattern

- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)
- [Linked List](../../../01.%20Data%20Structures/05.%20Linked%20List.md)
- [Design with Multiple Structures](../../../03.%20Problem%20Solving%20Patterns/11.%20Design%20with%20Multiple%20Structures.md)

## Approach

- Hash map: `key -> node`로 node를 즉시 찾는다.
- Doubly linked list: 사용 순서를 유지한다.
- head 쪽은 least recently used, tail 쪽은 most recently used로 둔다.
- 접근된 node는 tail 앞으로 이동한다.
- capacity 초과 시 head 다음 node를 제거한다.

## Python 3.14 Solution

```python
class Node:
    def __init__(self, key: int = 0, value: int = 0):
        self.key = key
        self.value = value
        self.prev: "Node | None" = None
        self.next: "Node | None" = None


class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache: dict[int, Node] = {}

        self.head = Node()
        self.tail = Node()
        self.head.next = self.tail
        self.tail.prev = self.head

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1

        node = self.cache[key]
        self._remove(node)
        self._add_to_recent(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            node = self.cache[key]
            node.value = value
            self._remove(node)
            self._add_to_recent(node)
            return

        node = Node(key, value)
        self.cache[key] = node
        self._add_to_recent(node)

        if len(self.cache) > self.capacity:
            lru = self.head.next
            if lru is None or lru is self.tail:
                return
            self._remove(lru)
            del self.cache[lru.key]

    def _remove(self, node: Node) -> None:
        prev_node = node.prev
        next_node = node.next
        if prev_node is not None:
            prev_node.next = next_node
        if next_node is not None:
            next_node.prev = prev_node

    def _add_to_recent(self, node: Node) -> None:
        prev_recent = self.tail.prev
        if prev_recent is not None:
            prev_recent.next = node
        node.prev = prev_recent
        node.next = self.tail
        self.tail.prev = node
```

## Correctness Notes

- Hash map은 key의 node를 `O(1)`에 찾게 해준다.
- Doubly linked list의 순서는 head에서 tail로 갈수록 최근 사용된 순서다.
- `get`과 기존 key의 `put`은 node를 tail 앞으로 옮기므로 최근 사용 상태가 반영된다.
- capacity 초과 시 head 다음 node는 가장 오래 사용되지 않은 node이므로 제거 대상이 맞다.

## Complexity

- `get`: `O(1)`
- `put`: `O(1)`
- Space: `O(capacity)`

## Mistake Notes

- dict만으로는 가장 오래된 key를 `O(1)`에 제거하기 어렵다.
- singly linked list는 중간 node 제거 시 previous를 바로 알 수 없어 불편하다.
- Python의 `OrderedDict`로도 구현 가능하지만, 자료구조 설계력은 직접 구현에서 드러난다.