# Grind 75 075. Kth Smallest Element in a BST

> BST의 inorder traversal은 값을 오름차순으로 방문한다. 따라서 inorder로 k번째 방문한 노드가 k번째로 작은 값이다.

## Link

- [LeetCode - Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst)

## Problem Model

BST property 때문에 왼쪽 subtree의 모든 값은 root보다 작고, 오른쪽 subtree의 모든 값은 root보다 크다. Inorder traversal은 `left, root, right`이므로 정렬 순서가 된다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Stack](../../../01.%20Data%20Structures/06.%20Stack.md)

## Approach

1. iterative inorder traversal을 수행한다.
2. 왼쪽으로 갈 수 있는 만큼 stack에 push한다.
3. stack에서 pop한 node가 다음으로 작은 값이다.
4. pop할 때마다 `k`를 줄인다.
5. `k == 0`이 되면 현재 node 값을 반환한다.

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
    def kthSmallest(self, root: TreeNode | None, k: int) -> int:
        stack: list[TreeNode] = []
        node = root

        while stack or node is not None:
            while node is not None:
                stack.append(node)
                node = node.left

            node = stack.pop()
            k -= 1
            if k == 0:
                return node.val

            node = node.right

        raise ValueError("k is larger than the number of nodes")
```

## Correctness Notes

- BST의 inorder traversal은 값을 오름차순으로 방문한다.
- stack은 아직 방문하지 않은 조상 node들을 보관하며, 다음 pop은 현재 남은 값 중 가장 작은 node다.
- pop 횟수는 오름차순 순위를 의미하므로 k번째 pop된 node가 k번째 smallest다.
- `k == 0`이 되는 순간 그 node 값을 반환하면 된다.

## Complexity

- Time: `O(h + k)`, where `h` is tree height.
- Space: `O(h)`

## Mistake Notes

- 전체 inorder list를 만들면 `O(n)` 공간이 든다. k번째까지만 탐색하면 더 효율적이다.
- 이 문제는 BST이므로 inorder 정렬 성질을 활용해야 한다.
- k는 1-indexed다.