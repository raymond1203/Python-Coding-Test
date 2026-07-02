# Grind 75 043. Serialize and Deserialize Binary Tree

> Tree 구조를 문자열로 바꾸고 다시 복원하는 설계 문제다. 값만 저장하면 구조가 사라지므로, `None` 자리까지 함께 기록해야 원래 tree를 정확히 되살릴 수 있다.

## Link

- [LeetCode - Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree)

## Problem Model

Binary tree를 저장 가능한 문자열로 직렬화하고, 같은 문자열에서 동일한 구조와 값을 가진 tree를 복원해야 한다. traversal 순서와 null marker가 포맷의 핵심이다.

## Pattern

- [Tree](../../../01.%20Data%20Structures/08.%20Tree.md)
- [DFS and BFS](../../../02.%20Algorithms/04.%20DFS%20and%20BFS.md)
- [Tree Traversal Patterns](../../../03.%20Problem%20Solving%20Patterns/14.%20Tree%20Traversal%20Patterns.md)
- [Recursive Divide and Conquer](../../../03.%20Problem%20Solving%20Patterns/15.%20Recursive%20Divide%20and%20Conquer.md)

## Approach

Preorder DFS를 사용한다.

1. node가 있으면 값을 기록한다.
2. node가 없으면 `#` marker를 기록한다.
3. 직렬화 순서는 `node, left, right`다.
4. 역직렬화도 같은 순서로 token을 소비하며 tree를 만든다.
5. `#`를 만나면 `None`을 반환한다.

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


class Codec:
    def serialize(self, root: TreeNode | None) -> str:
        tokens: list[str] = []

        def dfs(node: TreeNode | None) -> None:
            if node is None:
                tokens.append("#")
                return

            tokens.append(str(node.val))
            dfs(node.left)
            dfs(node.right)

        dfs(root)
        return ",".join(tokens)

    def deserialize(self, data: str) -> TreeNode | None:
        tokens: deque[str] = deque(data.split(","))

        def build() -> TreeNode | None:
            token = tokens.popleft()
            if token == "#":
                return None

            node = TreeNode(int(token))
            node.left = build()
            node.right = build()
            return node

        return build()
```

## Correctness Notes

- Preorder에서 각 node 값을 먼저 기록하고, 빈 child는 `#`로 기록하므로 구조 정보가 보존된다.
- deserialize는 serialize와 같은 preorder 순서로 token을 소비한다.
- `#` token은 해당 위치에 node가 없음을 정확히 나타낸다.
- 값 token을 만나면 node를 만들고 이어지는 token들이 각각 left subtree, right subtree를 구성한다.

## Complexity

- Time: `O(n)` for both serialize and deserialize.
- Space: `O(n)` for tokens and recursion stack in the worst case.

## Mistake Notes

- null marker 없이 값만 저장하면 서로 다른 구조의 tree가 같은 문자열이 될 수 있다.
- serialize와 deserialize의 traversal 순서가 반드시 일치해야 한다.
- LeetCode의 내부 포맷과 같을 필요는 없다. 두 함수가 서로 호환되면 된다.