# Grind 75 067. Construct Binary Tree from Preorder and Inorder Traversal

> Preorder와 inorder traversal 결과로 원래 binary tree를 복원하는 문제다. Preorder의 첫 값은 root이고, inorder에서 root를 기준으로 왼쪽/오른쪽 subtree가 나뉜다.

## Link

- [LeetCode - Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal)

## Problem Model

모든 값은 unique하다. Preorder는 `root, left, right`, inorder는 `left, root, right` 순서다. 이 두 순서 정보를 결합하면 tree 구조가 결정된다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)
- [Hash Table](../../../01.%20Data%20Structures/03.%20Hash%20Table.md)

## Approach

1. inorder 값의 index를 hash map으로 저장한다.
2. preorder를 앞에서부터 읽으며 root를 만든다.
3. 현재 inorder 범위 안에서 root 위치를 찾는다.
4. root 왼쪽 범위로 left subtree를 만들고, 오른쪽 범위로 right subtree를 만든다.
5. preorder index는 재귀 전체에서 공유한다.

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
    def buildTree(self, preorder: list[int], inorder: list[int]) -> TreeNode | None:
        inorder_index = {value: i for i, value in enumerate(inorder)}
        preorder_pos = 0

        def build(left: int, right: int) -> TreeNode | None:
            nonlocal preorder_pos
            if left > right:
                return None

            root_value = preorder[preorder_pos]
            preorder_pos += 1
            root = TreeNode(root_value)

            mid = inorder_index[root_value]
            root.left = build(left, mid - 1)
            root.right = build(mid + 1, right)
            return root

        return build(0, len(inorder) - 1)
```

## Correctness Notes

- preorder의 현재 첫 값은 현재 subtree의 root다.
- inorder에서 root 왼쪽은 left subtree, 오른쪽은 right subtree에 속한다.
- preorder는 root 다음에 left subtree의 preorder가 먼저 나오므로 left를 먼저 재귀 생성해야 한다.
- 각 값은 한 번 root가 되고 inorder 범위가 분할되므로 원래 tree가 복원된다.

## Complexity

- Time: `O(n)`
- Space: `O(n)` for index map and recursion stack.

## Mistake Notes

- inorder index를 매번 `list.index`로 찾으면 `O(n^2)`가 된다.
- left subtree를 먼저 만들어야 preorder 소비 순서가 맞다.
- 값이 unique하다는 조건이 중요하다.