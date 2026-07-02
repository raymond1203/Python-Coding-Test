# Grind 75 048. Diameter of Binary Tree

> Tree의 diameter는 두 노드 사이의 가장 긴 edge 수다. 각 노드에서 왼쪽 높이와 오른쪽 높이를 더하면 그 노드를 지나는 최장 경로 후보가 된다.

## Link

- [LeetCode - Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree)

## Problem Model

경로는 반드시 root를 지날 필요가 없다. 모든 노드에 대해 “그 노드를 최고점으로 하는 왼쪽 깊이 + 오른쪽 깊이”를 확인해야 한다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)

## Approach

1. postorder로 각 node의 height를 계산한다.
2. 왼쪽 height와 오른쪽 height의 합으로 diameter 후보를 갱신한다.
3. parent에게는 `max(left_height, right_height) + 1`만 반환한다.

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
    def diameterOfBinaryTree(self, root: TreeNode | None) -> int:
        diameter = 0

        def height(node: TreeNode | None) -> int:
            nonlocal diameter
            if node is None:
                return 0

            left_height = height(node.left)
            right_height = height(node.right)
            diameter = max(diameter, left_height + right_height)

            return max(left_height, right_height) + 1

        height(root)
        return diameter
```

## Correctness Notes

- `height(node)`는 node에서 leaf까지 내려가는 최대 node 수를 반환한다.
- 어떤 최장 경로도 어떤 노드를 기준으로 왼쪽 downward path와 오른쪽 downward path가 만나는 형태로 볼 수 있다.
- 각 노드에서 `left_height + right_height`를 확인하므로 모든 diameter 후보를 고려한다.
- parent에게는 한쪽 방향으로만 이어질 수 있으므로 최대 height만 반환한다.

## Complexity

- Time: `O(n)`
- Space: `O(h)`, recursion stack.

## Mistake Notes

- diameter는 node 수가 아니라 edge 수다. 위 height 정의에서는 `left_height + right_height`가 edge 수와 일치한다.
- 경로가 root를 지난다고 가정하면 틀린다.
- 높이 계산과 diameter 갱신을 한 traversal에서 처리해야 효율적이다.