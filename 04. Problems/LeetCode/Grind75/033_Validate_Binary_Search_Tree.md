# Grind 75 033. Validate Binary Search Tree

> 각 노드가 단순히 child와만 비교되는 것이 아니라, 조상들이 만든 허용 범위 안에 있어야 하는 문제다. BST 검증은 min/max boundary를 내려보내면 가장 안전하다.

## Link

- [LeetCode - Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree)

## Problem Model

BST에서는 어떤 노드의 왼쪽 subtree 모든 값이 node보다 작고, 오른쪽 subtree 모든 값이 node보다 커야 한다. 이 조건은 직계 child만 비교해서는 충분하지 않다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)

## Approach

1. 각 노드가 만족해야 하는 범위 `(low, high)`를 전달한다.
2. 현재 값이 `low < val < high`를 만족하지 않으면 invalid다.
3. 왼쪽 child에는 `(low, val)`을 전달한다.
4. 오른쪽 child에는 `(val, high)`를 전달한다.

## Python 3.14 Solution

```python
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
    def isValidBST(self, root: TreeNode | None) -> bool:
        def is_valid(node: TreeNode | None, low: float, high: float) -> bool:
            if node is None:
                return True
            if not low < node.val < high:
                return False
            return is_valid(node.left, low, node.val) and is_valid(node.right, node.val, high)

        return is_valid(root, float("-inf"), float("inf"))
```

## Correctness Notes

- `low`, `high`는 조상 노드들이 현재 subtree에 부여한 값의 허용 범위다.
- 현재 노드가 범위를 벗어나면 BST property를 위반한다.
- 왼쪽 subtree의 모든 값은 현재 값보다 작아야 하므로 upper bound가 현재 값으로 좁아진다.
- 오른쪽 subtree는 lower bound가 현재 값으로 좁아진다.

## Complexity

- Time: `O(n)`
- Space: `O(h)`, recursion stack.

## Mistake Notes

- child와만 비교하면 조상 제약을 놓친다.
- 중복 값은 허용되지 않으므로 `<`와 `>`의 strict inequality를 사용한다.
- inorder traversal로 증가 여부를 확인하는 풀이도 가능하다.