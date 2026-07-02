# Grind 75 023. Clone Graph

> 그래프의 구조를 그대로 복제하는 문제다. 핵심은 원본 노드에서 복제 노드로 가는 mapping을 유지해서 cycle과 중복 방문을 처리하는 것이다.

## Link

- [LeetCode - Clone Graph](https://leetcode.com/problems/clone-graph)

## Problem Model

각 노드는 값과 neighbor list를 가진다. 모든 reachable node를 새 객체로 만들고, neighbor 관계도 원본과 동일하게 연결해야 한다.

## Pattern

- [Graph](../../../01.%20Data%20Structures/09.%20Graph.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Graph Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/08.%20Graph%20Traversal%20Patterns.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)

## Approach

1. 입력 node가 없으면 `None`을 반환한다.
2. `clones[original] = cloned_node` mapping을 만든다.
3. BFS/DFS로 원본 그래프를 순회한다.
4. neighbor가 아직 복제되지 않았다면 복제하고 queue에 넣는다.
5. 현재 clone의 neighbor list에 neighbor clone을 추가한다.

## Python 3.14 Solution

```python
from collections import deque


class Node:
    def __init__(self, val: int = 0, neighbors: list["Node"] | None = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []


class Solution:
    def cloneGraph(self, node: Node | None) -> Node | None:
        if node is None:
            return None

        clones: dict[Node, Node] = {node: Node(node.val)}
        queue: deque[Node] = deque([node])

        while queue:
            current = queue.popleft()

            for neighbor in current.neighbors:
                if neighbor not in clones:
                    clones[neighbor] = Node(neighbor.val)
                    queue.append(neighbor)

                clones[current].neighbors.append(clones[neighbor])

        return clones[node]
```

## Correctness Notes

- `clones`는 방문 여부와 원본-복제 대응 관계를 동시에 나타낸다.
- 각 원본 노드는 정확히 하나의 복제 노드에 대응한다.
- 모든 edge `(current, neighbor)`를 볼 때 복제 그래프에도 `clone(current) -> clone(neighbor)`를 추가한다.
- reachable한 모든 노드는 traversal 중 발견되므로 전체 component가 복제된다.

## Complexity

- Time: `O(V + E)`
- Space: `O(V)`

## Mistake Notes

- 값 `val`만 key로 쓰면 안 된다. 노드 객체 자체를 key로 써야 구조를 정확히 보존한다.
- cycle이 있으므로 mapping 없이 재귀 생성하면 무한 루프가 날 수 있다.
- neighbor 연결은 “복제 노드끼리” 해야 한다. 원본 neighbor를 섞으면 deep copy가 아니다.