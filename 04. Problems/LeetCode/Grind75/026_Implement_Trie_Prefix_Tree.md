# Grind 75 026. Implement Trie (Prefix Tree)

> 문자열 집합을 prefix 기준으로 저장하는 자료구조 설계 문제다. 각 문자 간선을 따라 내려가며 단어 종료 여부를 별도로 표시한다.

## Link

- [LeetCode - Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree)

## Problem Model

Trie는 root에서 시작해 문자 하나씩 edge를 따라 내려가는 tree다. 같은 prefix를 가진 단어들은 앞부분 경로를 공유한다.

## Pattern

- [Trie](../../../01.%20Data%20Structures/11.%20Trie.md)
- [Trie Prefix Search](../../../03.%20Problem%20Solving%20Patterns/10.%20Trie%20Prefix%20Search.md)
- [Design with Multiple Structures](../../../03.%20Problem%20Solving%20Patterns/11.%20Design%20with%20Multiple%20Structures.md)

## Approach

- `insert`: 문자마다 child node를 만들거나 따라가고, 마지막 node에 `is_word = True`를 표시한다.
- `search`: 전체 단어 경로가 존재하고 마지막 node가 단어 종료여야 한다.
- `startsWith`: prefix 경로만 존재하면 된다.

## Python 3.14 Solution

```python
class TrieNode:
    def __init__(self):
        self.children: dict[str, "TrieNode"] = {}
        self.is_word = False


class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_word = True

    def search(self, word: str) -> bool:
        node = self._find_node(word)
        return node is not None and node.is_word

    def startsWith(self, prefix: str) -> bool:
        return self._find_node(prefix) is not None

    def _find_node(self, text: str) -> TrieNode | None:
        node = self.root
        for char in text:
            if char not in node.children:
                return None
            node = node.children[char]
        return node
```

## Correctness Notes

- `insert`는 word의 모든 prefix 경로를 trie 안에 만든다.
- 마지막 node의 `is_word`는 “이 prefix가 실제 단어로 끝나는가”를 구분한다.
- `search`는 경로 존재와 단어 종료 여부를 모두 요구하므로 prefix만 있는 경우를 걸러낸다.
- `startsWith`는 경로 존재만 확인하므로 prefix 검색 조건과 일치한다.

## Complexity

- `insert`: `O(L)`
- `search`: `O(L)`
- `startsWith`: `O(L)`
- Space: inserted characters total에 비례한다.

## Mistake Notes

- `search("app")`와 `startsWith("app")`는 다르다. `is_word`를 반드시 둬야 한다.
- node마다 26칸 배열을 써도 되지만, Python에서는 dict가 가독성과 범용성이 좋다.
- 빈 root 자체는 문자를 뜻하지 않는 sentinel node로 보는 것이 깔끔하다.