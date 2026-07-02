# Grind 75 011. Lowest Common Ancestor of a Binary Search Tree

> BST에서는 값의 대소 관계가 곧 이동 방향이다. 두 노드가 현재 노드를 기준으로 갈라지는 첫 지점이 LCA다.

## Link

- [LeetCode - Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree)

## Problem Model

BST에서 두 노드 `p`, `q`의 lowest common ancestor를 찾는다. 일반 binary tree LCA와 달리 subtree를 모두 탐색할 필요가 없다. BST property를 이용하면 한 방향으로만 내려가면 된다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)

## Approach

1. `p.val`, `q.val`의 작은 값과 큰 값을 구한다.
2. 현재 노드 값이 둘보다 크면 두 노드는 모두 왼쪽 subtree에 있다.
3. 현재 노드 값이 둘보다 작으면 두 노드는 모두 오른쪽 subtree에 있다.
4. 그 외에는 현재 노드가 둘 사이에 있거나 둘 중 하나와 같으므로 LCA다.

## Python 3.14 Solution

```python
class TreeNode:
    def __init__(
        self,
        x: int,
        left: "TreeNode | None" = None,
        right: "TreeNode | None" = None,
    ):
        self.val = x
        self.left = left
        self.right = right


class Solution:
    def lowestCommonAncestor(
        self,
        root: TreeNode,
        p: TreeNode,
        q: TreeNode,
    ) -> TreeNode:
        low, high = sorted((p.val, q.val))
        node: TreeNode | None = root

        while node is not None:
            if node.val > high:
                node = node.left
            elif node.val < low:
                node = node.right
            else:
                return node

        raise ValueError("p and q must belong to the BST")
```

## Correctness Notes

- 현재 노드 값이 `p`, `q`보다 크면 BST property상 두 노드는 왼쪽 subtree에만 있을 수 있다.
- 현재 노드 값이 `p`, `q`보다 작으면 두 노드는 오른쪽 subtree에만 있을 수 있다.
- 두 값이 현재 노드를 기준으로 갈라지거나 현재 노드가 둘 중 하나라면, 현재 노드는 두 노드를 모두 포함하는 가장 낮은 지점이다.

## Complexity

- Time: `O(h)`, where `h` is tree height.
- Space: `O(1)`

## Mistake Notes

- 이 문제는 BST다. 일반 binary tree처럼 양쪽 subtree를 전부 탐색하면 조건을 활용하지 못한다.
- `p`와 `q` 중 하나가 ancestor일 수 있다. 이때 그 ancestor 자신이 LCA다.
- `low`, `high`를 미리 정하면 `p`, `q`의 순서에 영향을 받지 않는다.

## Related Notes

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Binary Search](../../../02.%20Algorithms/02.%20Binary%20Search.md)
