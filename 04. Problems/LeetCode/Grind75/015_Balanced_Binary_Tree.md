# Grind 75 015. Balanced Binary Tree

> 각 노드에서 왼쪽/오른쪽 subtree의 높이 차이가 1 이하인지 확인하는 문제다. 핵심은 높이 계산과 균형 여부 판단을 한 번의 postorder traversal로 합치는 것이다.

## Link

- [LeetCode - Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree)

## Problem Model

Binary tree가 height-balanced인지 판단한다. 어떤 노드라도 왼쪽 subtree와 오른쪽 subtree의 높이 차이가 1을 넘으면 전체 tree는 balanced가 아니다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)

## Approach

1. 각 노드에서 subtree height를 계산한다.
2. 왼쪽 subtree가 이미 불균형이면 즉시 실패 신호 `-1`을 올린다.
3. 오른쪽 subtree도 같은 방식으로 확인한다.
4. 두 높이 차이가 1보다 크면 `-1`을 반환한다.
5. 균형이면 `max(left_height, right_height) + 1`을 반환한다.

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
    def isBalanced(self, root: TreeNode | None) -> bool:
        def height_or_fail(node: TreeNode | None) -> int:
            if node is None:
                return 0

            left_height = height_or_fail(node.left)
            if left_height == -1:
                return -1

            right_height = height_or_fail(node.right)
            if right_height == -1:
                return -1

            if abs(left_height - right_height) > 1:
                return -1

            return max(left_height, right_height) + 1

        return height_or_fail(root) != -1
```

## Correctness Notes

- `height_or_fail(node)`는 subtree가 balanced이면 높이를, 아니면 `-1`을 반환한다.
- leaf부터 위로 올라오는 postorder 구조이므로 현재 노드는 두 child subtree의 정확한 높이를 알고 판단한다.
- 어떤 subtree에서든 `-1`이 발생하면 그 subtree를 포함하는 전체 tree도 balanced가 아니므로 즉시 전파해도 안전하다.

## Complexity

- Time: `O(n)`
- Space: `O(h)`, recursion stack where `h` is tree height.

## Mistake Notes

- 각 노드마다 별도로 height를 다시 계산하면 최악의 경우 `O(n^2)`가 된다.
- 균형 조건은 “root의 양쪽 높이 차이”만이 아니라 모든 노드에 대해 성립해야 한다.
- height와 balanced 여부를 별도 traversal로 나누기보다 sentinel 값을 쓰면 깔끔하다.