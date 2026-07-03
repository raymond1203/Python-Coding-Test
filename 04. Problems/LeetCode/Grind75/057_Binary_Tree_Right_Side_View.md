# Grind 75 057. Binary Tree Right Side View

> Binary tree를 오른쪽에서 봤을 때 보이는 노드들을 depth 순서로 반환하는 문제다. 각 level에서 가장 오른쪽에 있는 노드를 선택하면 된다.

## Link

- [LeetCode - Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)

## Problem Model

같은 depth에 여러 노드가 있으면 가장 오른쪽 노드만 보인다. 따라서 level order traversal을 하며 level의 마지막 노드를 기록하면 된다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Queue and Deque](../../../01.%20Data%20Structures/07.%20Queue%20and%20Deque.md)

## Approach

1. root가 없으면 빈 list를 반환한다.
2. BFS queue에 root를 넣는다.
3. 각 level마다 현재 queue 크기를 고정한다.
4. level 안에서 마지막으로 pop되는 노드의 값을 결과에 추가한다.
5. child는 왼쪽, 오른쪽 순서로 넣어도 level의 마지막 노드는 오른쪽 시야 기준 노드가 된다.

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
    def rightSideView(self, root: TreeNode | None) -> list[int]:
        if root is None:
            return []

        result: list[int] = []
        queue: deque[TreeNode] = deque([root])

        while queue:
            level_size = len(queue)
            for i in range(level_size):
                node = queue.popleft()
                if i == level_size - 1:
                    result.append(node.val)

                if node.left is not None:
                    queue.append(node.left)
                if node.right is not None:
                    queue.append(node.right)

        return result
```

## Correctness Notes

- BFS의 각 반복 시작 시 queue에는 같은 depth의 노드들만 있다.
- 왼쪽 child를 먼저 넣고 오른쪽 child를 나중에 넣으면 같은 level에서 오른쪽에 가까운 노드가 뒤쪽에 위치한다.
- 각 level의 마지막 노드는 오른쪽에서 보이는 노드다.
- 모든 level을 처리하므로 오른쪽 시야 결과가 depth 순서로 완성된다.

## Complexity

- Time: `O(n)`
- Space: `O(w)`, where `w` is maximum tree width.

## Mistake Notes

- 전체 tree의 가장 오른쪽 경로만 따라가면 안 된다. 오른쪽 child가 없는 level에서는 왼쪽 subtree의 노드가 보일 수 있다.
- level boundary를 위해 `level_size`를 먼저 고정해야 한다.