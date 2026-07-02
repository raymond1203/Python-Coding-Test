# Grind 75 022. Binary Tree Level Order Traversal

> Binary tree를 깊이별로 묶어 출력하는 문제다. BFS에서 queue의 현재 길이를 한 level의 크기로 고정하면 level 단위 처리가 자연스럽다.

## Link

- [LeetCode - Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal)

## Problem Model

Root에서 가까운 노드부터 방문하되, 같은 depth에 있는 노드들을 하나의 list로 묶어야 한다. Tree를 unweighted graph로 보면 BFS layer가 곧 depth다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)

## Approach

1. root가 없으면 빈 list를 반환한다.
2. queue에 root를 넣는다.
3. 반복마다 현재 queue 길이만큼만 pop하여 하나의 level을 만든다.
4. 각 노드의 child를 다음 level 후보로 queue에 넣는다.
5. level list를 결과에 추가한다.

## Python 3.14 Solution

```python
from collections import deque


class TreeNode:
    def __init__(
        self,
        val: int = 0,
        left: "TreeNode | None" = None,
        right: "TreeNode | None" = None,
    ):
        self.val = val
        self.left = left
        self.right = right


class Solution:
    def levelOrder(self, root: TreeNode | None) -> list[list[int]]:
        if root is None:
            return []

        result: list[list[int]] = []
        queue: deque[TreeNode] = deque([root])

        while queue:
            level_size = len(queue)
            level: list[int] = []

            for _ in range(level_size):
                node = queue.popleft()
                level.append(node.val)

                if node.left is not None:
                    queue.append(node.left)
                if node.right is not None:
                    queue.append(node.right)

            result.append(level)

        return result
```

## Correctness Notes

- 반복 시작 시 queue에는 같은 depth의 노드들만 들어 있다.
- `level_size`를 먼저 고정하므로, 이번 반복 중 추가되는 child들은 다음 level에서 처리된다.
- BFS는 root로부터 거리 순서대로 확장하므로 결과는 depth 순서와 일치한다.

## Complexity

- Time: `O(n)`
- Space: `O(w)`, where `w` is maximum tree width.

## Mistake Notes

- `for _ in range(len(queue))`처럼 매번 동적으로 길이를 읽으면 level 경계가 깨질 수 있다. 시작 시점의 `level_size`를 저장하자.
- DFS로도 depth index를 넘겨 풀 수 있지만, level order의 기본형은 BFS가 가장 직관적이다.