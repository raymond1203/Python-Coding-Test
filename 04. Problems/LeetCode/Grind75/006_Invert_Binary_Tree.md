# Grind 75 006. Invert Binary Tree

> 각 노드에서 왼쪽과 오른쪽 자식을 바꾸고, 같은 작업을 서브트리에 반복하는 Tree recursion 문제다.

## Link

- [LeetCode - Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree)

## Problem Model

Tree 전체를 뒤집는다는 것은 모든 노드에서 `left`와 `right`를 교환한다는 뜻이다. 현재 노드에서 swap한 뒤 양쪽 서브트리에 같은 연산을 적용하면 된다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)

## Approach

1. `root is None`이면 그대로 반환한다.
2. 현재 노드의 왼쪽과 오른쪽을 교환한다.
3. 교환된 양쪽 자식에 대해 재귀 호출한다.
4. root를 반환한다.

## Python 3.14 Solution

```python
from __future__ import annotations
from dataclasses import dataclass

@dataclass
class TreeNode:
    val: int = 0
    left: TreeNode | None = None
    right: TreeNode | None = None


class Solution:
    def invertTree(self, root: TreeNode | None) -> TreeNode | None:
        if root is None:
            return None

        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

## Complexity

- Time: O(n)
- Space: O(h), recursion stack

## Mistake Points

- swap 전후 어느 쪽에 재귀를 호출해도 전체 노드를 한 번씩 방문하면 된다.
- 반환값은 새 tree가 아니라 같은 root reference다.
- skewed tree에서는 recursion depth가 O(n)이 될 수 있다.

## Related Notes

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Recursion](../../../02.%20Algorithms/03.%20Recursion.md)
