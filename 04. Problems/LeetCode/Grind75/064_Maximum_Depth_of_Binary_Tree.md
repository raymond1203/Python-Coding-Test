# Grind 75 064. Maximum Depth of Binary Tree

> Binary tree의 최대 깊이를 구하는 기본 재귀 문제다. 각 노드는 왼쪽 subtree 깊이와 오른쪽 subtree 깊이 중 더 큰 값에 1을 더해 부모에게 반환한다.

## Link

- [LeetCode - Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree)

## Problem Model

Tree의 depth는 root에서 가장 먼 leaf까지의 node 수다. 빈 tree의 깊이는 0이다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Recursion](../../../02.%20Algorithms/03.%20Recursion.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)

## Approach

1. node가 `None`이면 깊이는 0이다.
2. 왼쪽 subtree의 최대 깊이를 구한다.
3. 오른쪽 subtree의 최대 깊이를 구한다.
4. 둘 중 큰 값에 현재 node 1을 더한다.

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
    def maxDepth(self, root: TreeNode | None) -> int:
        if root is None:
            return 0
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```

## Correctness Notes

- 빈 subtree의 depth는 0이다.
- root를 포함한 최대 깊이는 root에서 더 깊은 child subtree로 내려가는 경로다.
- 각 subtree가 자신의 최대 깊이를 정확히 반환한다고 하면, 현재 node의 깊이는 `1 + max(left, right)`이다.
- 재귀가 모든 node에 대해 이 관계를 적용하므로 전체 최대 깊이가 계산된다.

## Complexity

- Time: `O(n)`
- Space: `O(h)`, recursion stack.

## Mistake Notes

- edge 수가 아니라 node 수 기준 depth다.
- skewed tree에서는 recursion depth가 `O(n)`까지 갈 수 있다.