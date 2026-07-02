# Grind 75 040. Lowest Common Ancestor of a Binary Tree

> 일반 binary tree에서는 BST처럼 값의 대소 관계를 사용할 수 없다. 대신 각 subtree가 `p` 또는 `q`를 포함하는지 재귀적으로 묻고, 양쪽에서 하나씩 발견되는 첫 노드가 LCA다.

## Link

- [LeetCode - Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree)

## Problem Model

두 노드 `p`, `q`를 모두 descendant로 갖는 가장 낮은 노드를 찾아야 한다. 한 노드가 다른 노드의 ancestor인 경우 그 노드 자신이 LCA다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)

## Approach

1. 현재 노드가 `None`, `p`, `q` 중 하나라면 그대로 반환한다.
2. 왼쪽 subtree와 오른쪽 subtree를 재귀 탐색한다.
3. 양쪽에서 모두 값이 오면 현재 노드가 LCA다.
4. 한쪽만 값이 오면 그 값을 위로 올린다.

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
        root: TreeNode | None,
        p: TreeNode,
        q: TreeNode,
    ) -> TreeNode | None:
        if root is None or root is p or root is q:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left is not None and right is not None:
            return root
        return left if left is not None else right
```

## Correctness Notes

- 재귀 반환값은 현재 subtree 안에서 발견한 `p`, `q`, 또는 이미 결정된 LCA를 의미한다.
- 왼쪽과 오른쪽에서 각각 값이 발견되면 `p`, `q`가 현재 노드를 기준으로 갈라지므로 현재 노드가 가장 낮은 공통 조상이다.
- 한쪽에서만 값이 발견되면 두 노드가 모두 그쪽에 있거나 하나만 발견된 상태이므로 그 값을 위로 전달한다.
- `root is p` 또는 `root is q`인 경우, 다른 노드가 그 아래에 있다면 이 노드가 LCA가 될 수 있다.

## Complexity

- Time: `O(n)`
- Space: `O(h)`, recursion stack.

## Mistake Notes

- 이 문제는 BST가 아니므로 값 비교로 방향을 정하면 안 된다.
- 노드 값이 아니라 노드 객체 자체를 비교하는 것이 안전하다.
- 한 노드가 다른 노드의 ancestor인 케이스를 base case가 자연스럽게 처리한다.